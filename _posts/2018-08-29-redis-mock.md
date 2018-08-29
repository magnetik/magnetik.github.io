---
title: Use decorator to mock redis connection in Symfony
desc: 
---

To use redis conveniently in my Symfony projects, I use [Snc Redis Bundle](https://github.com/snc/SncRedisBundle) that automagically defines the service `snc_redis.default_client` which is injected as arguments in other service.
But here comes the functional tests, and the redis server is not up, or not in a "clean" state.

[RedisMock](https://github.com/M6Web/RedisMock) from M6Web can helps by mocking the redis functions but I needed to replace the snc bundle services in the test env only.

I've found decorating the snc service in `services_test.xml` convenient:

        <service
            id="snc_redis.default_client.mock"
            class="M6Web\Component\RedisMock\RedisMock"
            decorates="snc_redis.default_client"
        >
            <factory service="M6Web\Component\RedisMock\RedisMockFactory" method="getAdapter"/>
            <argument>Predis\Client</argument>

        </service>
