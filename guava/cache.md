# Cache
<!-- toc -->

```java
public class Caches {

    @Test
    public void cacheTest() throws InterruptedException {
        String k = "1";
        LoadingCache<String, Set<Integer>> caches = CacheBuilder.newBuilder()
                .maximumSize(1000)
                .expireAfterWrite(1, TimeUnit.SECONDS)
                .removalListener(notification -> System.out.println("remove: " + notification))
                .build(new CacheLoader<String, Set<Integer>>() {
                    @Override
                    public Set load(String key) {
                        if (Objects.equals(key, k)) {
                            return Sets.newHashSet(1, 2);
                        }
                        return null;
                    }
                });
        try {
            System.out.println(caches.get(k));
            caches.put("2", Sets.newHashSet(3, 4));
            TimeUnit.SECONDS.sleep(2);
            System.out.println(caches.get(k));
            // 抛 CacheLoader.InvalidCacheLoadException 异常
            // System.out.println(caches.get("2"));

            // get 不到就调用 后面的函数进行 set
            Set<Integer> set = caches.get("2", () -> Sets.newHashSet(3, 4));
            System.out.println(set);
            System.out.println(caches.get("2"));
        } catch (ExecutionException e) {
            e.printStackTrace();
        } catch (CacheLoader.InvalidCacheLoadException e) {
            System.out.println("load null");
        }
    }

    @Test
    public void  testRefresh() throws InterruptedException, ExecutionException {
        String k = "1";
        LoadingCache<String, Integer> caches = CacheBuilder.newBuilder()
                .maximumSize(1000)
                .refreshAfterWrite(1, TimeUnit.SECONDS)
                .removalListener(notification -> System.out.println("remove: " + notification))
                .build(new CacheLoader<String, Integer>() {
                    private int count = 0;
                    @Override
                    public Integer load(String key) throws Exception {
                        System.out.println("load key: " + key);
                        return count + Integer.parseInt(key);
                    }

                    @Override
                    public ListenableFuture<Integer> reload(String key, Integer oldValue)
                        throws Exception {
                        System.out.println("reload key: " + key);
                        count++;
                        return super.reload(key, oldValue);
                    }
                });

        TimeUnit.SECONDS.sleep(2);
        System.out.println(caches.get(k));
        TimeUnit.SECONDS.sleep(3);
        System.out.println(caches.get(k));
        System.out.println(caches.get(k));
    }
}
```
