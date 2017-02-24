---
title: Rückgabewerte schnell und einfach cachen
date: 2012-03-12
tags:
  - c#
  - caching
---
    public class UserService
    {
         public User GetUserById(Guid id)
         {
             var cacheKey =
                 string.Concat(
                     "UserService.GetUserById.",
                     id,
                     ".",
                     Thread.CurrentThread.CurrentUICulture.Name);
    
             var user = Cache.Get<User>(cacheKey);
             if (user == null)
             {
                 user = _repository.Get(id);
    
                 Cache.Set(cacheKey, user);
             }
    
             return user;
         }
    }

Viele Zeilen der Methode `GetUserById` entfallen in diesem Beispiel auf das obligatorische Cachegehampel (Zusammensetzen eines eindeutigen Cache-Keys, Instanz im Cache suchen, Instanz im Cache ablegen). Die eigentliche Logik ist recht überschaubar.

Was kann man also tun, um diesen *Overhead* zu reduzieren?

Eine Möglichkeit wäre z.B. das AOP-Framework [PostSharp](http://www.sharpcrafters.com/) und ein passender [Cache-Aspekt](http://www.sharpcrafters.com/blog/post/solid-caching.aspx). Finde ich sehr interessant und werde ich mir bei Gelegenheit definitiv näher ansehen.

Alternativ kann ein *ähnliches Verhalten* mit [Unity](http://unity.codeplex.com/) oder [Castle Windsor](http://stw.castleproject.org/Default.aspx) und deren Interceptoren erreicht werden.

"Meine Lösung" sieht es momentan vor, dass ich den [MemoryCache](http://msdn.microsoft.com/en-us/library/system.runtime.caching.memorycache.aspx) (seit .NET 4.0 Bestandteil des Frameworks) und eine [Extension Method](http://msdn.microsoft.com/en-us/library/bb383977.aspx) verwende. Aus der oben gezeigten Klasse wird:

    public class UserService
    {
         public User GetUserById(Guid id)
         {
             return _repository.Get(id);
         }
    }

Der Zugriff auf die Methode könnte nun wie folgt aussehen:

    var user = MemoryCache.Default.ValueFor(() => _userService.GetUserById(id));

`ValueFor()`-Extension Method:

    public static class ObjectCacheExtensions
    {
        public static T ValueFor<T>(
           this ObjectCache cache,
           Expression<Func<T>> expression,
           CacheItemPolicy policy = null)
        {
            var call = expression.Body as MethodCallExpression;
            if (call == null)
            {
                throw new InvalidOperationException();
            }
    
            var declaringType = call.Method.DeclaringType;
            if (declaringType == null)
            {
                // Lambda?
                throw new InvalidOperationException();
            }
    
            var type = declaringType.Name;
            var method = call.Method.Name;
    
            var arguments =
                string.Join(
                    "_",
                    call.Arguments.Select(
                        x =>
                        Expression.Lambda(x).Compile().DynamicInvoke() ?? x));
    
            var culture = Thread.CurrentThread.CurrentUICulture.Name;
    
            var key =
                string.Concat(type, "_", method, "_", arguments, "_", culture);
    
            var obj = cache[key];
            if (obj == null)
            {
                if (policy == null)
                {
                    // Default cache item policy
    
                    policy =
                        new CacheItemPolicy
                        {
                            Priority = CacheItemPriority.Default,
                            AbsoluteExpiration = DateTime.Now.AddMinutes(10)
                        };
                }
    
                var function = expression.Compile();
                obj = function();
    
                if (obj != null)
                {
                    cache.Add(new CacheItem(key, obj), policy);
                }
            }
    
            return (T)obj;
        }
    }

Innerhalb der Extension Method wird nun ein Cache-Key auf Basis von Typname, Methodenname, Parametern und der aktuellen UI-Culture generiert. Wird das dazugehörige Objekt im Cache gefunden, wird es ohne Umwege zurückgegeben. Falls nicht, wird die übergebene Expression ausgeführt und dessen Rückgabewert unter dem generierten Cache-Key abgelegt.
