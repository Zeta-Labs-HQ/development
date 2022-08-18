Plugins could have a custom exception that is propagated to the `on_error` handler in the respective plugin.

This would provide the following details:
- Plugin name
- Error source
- Error message and whole traceback
