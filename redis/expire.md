### Expiry

* Set a timeout on key. After the timeout has expired, the key will automatically be deleted.
    ```
    SETEX key ttl value
    ```
* The timeout will only be cleared by commands that delete or overwrite the contents of the key, including DEL, SET, GETSET and all the *STORE commands. This means that all the operations that conceptually alter the value stored at the key without replacing it with a new one will leave the timeout untouched.
* The timeout can also be cleared, turning the key back into a persistent key, using the PERSIST command. PERSIST could be used to persist all the keys in the redis.
    ```
    PERSSIT key
    ```
* If a key is overwritten by RENAME, like in the case of an existing key Key_A that is overwritten by a call like RENAME Key_B Key_A, it does not matter if the original Key_A had a timeout associated or not, the new key Key_A will inherit all the characteristics of Key_B.
    ```
    RENAME oldkey newkey
    ```
* Expire could be used to get set expiry time for the keys within redis.
    ```
    EXPIRE key 100
    ```
* Keys expiring information is stored as absolute Unix timestamps. This means that the time is flowing even when the Redis instance is not active.For expires to work well, the computer time must be taken stable. If you move an RDB file from two computers with a big desync in their clocks, funny things may happen.

* Redis keys are expired in two ways: a passive way, and an active way. A key is passively expired simply when some client tries to access it, and the key is found to be timed out. periodically Redis tests a few keys at random among keys with an expire set. All the keys that are already expired are deleted from the keyspace.
* Specifically this is what Redis does 10 times per second:
    1. Test 20 random keys from the set of keys with an associated expire.
    1. Delete all the keys found expired.
    1. If more than 25% of keys were expired, start again from step 1.

* In order to obtain a correct behavior without sacrificing consistency, when a key expires, a DEL operation is synthesized in both the AOF file and gains all the attached replicas nodes. This way the expiration process is centralized in the master instance, and there is no chance of consistency errors.
