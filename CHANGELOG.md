# Changelog

## Unreleased (1.x)

### Fixed
- An unreachable matchID no longer carries the token: the `UnavailableException` wrapped the HTTP
  client's own exception, whose trace kept the request's headers in its arguments
  (`zend.exception_ignore_args` off). The previous is now a copy of it (the same class and code, the
  token masked in its message) and no trace of the chain keeps its arguments. Needs
  omnisense/omnistate's `Http\Redaction`.
