# Plugin system

## Why?

Plugin system is a way to extend Riemann with new functionality. Every bot differs, and has a different way
to work with things, especially external services, custom internal wrappers, etc.

This empowers and creates new way to enable _anyone_ make their own plugins with the consistent API provided
by `riemann-plugins` repository.

## Getting started

There would be an abstract class with the API in order to ensure the functions and hooks to be called are
consistent.

```py
from riemann.plugins import PluginBase, PluginException, PluginSettings

class MyPlugin(PluginBase):
    def __init__(self, name: str, *args, **kwargs) -> None:
        super().__init__(*args, **kwargs)

        # Hook to the `name` property in app state (the bot)
        self.app.hook("name", name)

    # Expose a property with settings. Maybe abstract? Return a default value if not overridden.
    @property
    def settings(self) -> PluginSettings:
        return PluginSettings()

    # Anything that's event: `on_{event_name}`
    def on_startup(self) -> None:
        self.logger.info("MyPlugin started")

    def on_shutdown(self) -> None:
        self.logger.info("MyPlugin stopped")

    # Error handler hook. All the errors caused in app propagated here.
    # Figure out how to to pass all errors for the specific app here?
    def on_error(self, error: PluginException) -> None:
        self.logger.error(error)
```

And, then assuming there's the riemann app initialized, you can load a plugin as so:

```py
from plugins import MyPlugin

app.load_plugin("hello-world", MyPlugin("name"))
```

You can enable and disable a plugin dynamically (comes with side-effects due to app-hooking):

```py
app.enable_plugin("hello-world")  # This adds the `.name` property.

app.disable_plugin("hello-world")  # This removes the `.name` property.
```
