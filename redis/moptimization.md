### Memory optimisation

* Using 32 bit instances
  * Redis compiled with 32 bit target uses a lot less memory per key, since pointers are small, but such an instance will be limited to 4 GB of maximum memory usage. To compile Redis as 32 bit binary use make 32bit. RDB and AOF files are compatible between 32 bit and 64 bit instances (and between little and big endian of course) so you can switch from 32 to 64 bit, or the contrary, without problems.
  * https://stackoverflow.com/questions/9625246/what-are-the-underlying-data-structures-used-for-redis
  * https://medium.com/@kousiknath/a-little-internal-on-redis-key-value-storage-implementation-fdf96bac7453#:~:text=Each%20redis%20database%20instance%20(%20databases,saved%20inside%20the%20hash%20tables.