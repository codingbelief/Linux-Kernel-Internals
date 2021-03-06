
Introduction of caching snoop protocols

Attaching a cache to each CPU increases performance in many ways. Bringing memory closer to the CPU reduces the average memory access time and at the same time reducing the bandwidth load on the memory bus. The challenge with adding cache to each CPU in a shared memory architecture is that it allows multiple copies of a memory block to exist. This is called the cache-coherency problem. To solve this, caching snoop protocols were invented attempting to create a model that provided the correct data while not trying to eat up all the bandwidth on the bus. The most popular protocol, write invalidate, erases all other copies of data before writing the local cache. Any subsequent read of this data by other processors will detect a cache miss in their local cache and will be serviced from the cache of another CPU containing the most recently modified data. This model saved a lot of bus bandwidth and allowed for Uniform Memory Access systems to emerge in the early 1990s. Modern cache coherency protocols are covered in more detail by part 3.

SMP is an acronym for “Symmetric Multi-Processor”. It describes a design in which two or more identical CPU cores share access to main memory.

The cache behavior becomes relevant to this discussion when each CPU core has its own private cache. In a simple model, the caches have no way to interact with each other directly. The values held by core #1’s cache are not shared with or visible to core #2’s cache except as loads or stores from main memory. The long latencies on memory accesses would make inter-thread interactions sluggish, so it’s useful to define a way for the caches to share data. This sharing is called cache coherency, and the coherency rules are defined by the CPU architecture’s cache consistency model.

Observability

Before going further, it’s useful to define in a more rigorous fashion what is meant by “observing” a load or store. Suppose core 1 executes “A = 1”. The store is initiated when the CPU executes the instruction. At some point later, possibly through cache coherence activity, the store is observed by core 2. In a write-through cache it doesn’t really complete until the store arrives in main memory, but the memory consistency model doesn’t dictate when something completes, just when it can be observed.

