# Edge Arch

[w3 edge arch](https://www.w3.org/TR/edge-arch/)

Spec Implemented

- [ ] no-store
- [ ] no-store-remote
- [x] max-age
- [ ] content
- [ ] Surrogate-Control Targeting

Added, x-protected

### Added behavior for max-age freshness

The max-age freshness extension is used to indicate that the cache, while acting as a surrogate, will deliver a push promise to be updated when the cache updates its value.  It will guarantee to deliver that promise for the max-age freshness extension (the +x).

Because of this, the response generated by the cache will have a stale-while-revalidate value equal to the freshness extension.

The push-promise will have the cache-control header "no-cache" to indicate it should not be cancelled from the client, if the client already has a response for that value cached.

The push promise will have a prefer header saying when it prefers the response, and indicating it wants the result only when it is modified from what is delivered.

example:  response from origin with `surrogate-control: max-age=5+100` will result in 
          1) response from cache with `cache-control: stale-while-revalidate=100`
          2) push promise with `prefer: wait=100` and `cache-control: no-cache`

Note: this is only implemented for cache-able requests ("GET" with no body) and cache-able responses
# x-protected

### Introduction

HTTP Caching provides response cache-control directives private and public.  A shared cache may also want to share a response to a subset of users based on authorized roles or permissions.

This document defines the new behavioral response surrogate-control directive x-protected. The prefix x- is added to indicate it is non-standard.  X-protected indicates that a cache response may be shared with any request whose matches the same authorization permissions as the original request that cached the response.  This requires the cache to be-able to distinguish and identify authorization permissions with any request matching the authorization permissions associated with the original request that cached the response

When present in an HTTP response, the x-protected Cache-Control extension indicates that caches MAY serve the response to any request which has an authorization scope identical to the original request that caused it to be cached.

The cache MUST ignore the x-protected Cache-Control header if it has no knowledge or way of determining what authorization roles/permissions a request had.
