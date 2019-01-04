## Data versioning

### Immutability of persisted objects

- Caching - `Get` requests for a specific version of an object is idempotent and can be cached at a various layers
- Reproducibility - By ensuing that data used as inputs to a computation can never be modified, the computations can be re-executed and guarantee identical results.
- Concurrency - Reading specific versions is safe without the need for locking. 
- Traceability - Modification history is never lost.
- Performance - Inserts are handled better than updates.


