Each plugin could have it's own independent settings and constraints to make it much more flexible.

The settings would be a dataclass, with default values to use that if the user doesn't override the config.

```py
from dataclasses import dataclass

class PluginSettings:
    single_instance: bool = False  # If true, only one instance of the plugin can be loaded.

    @classmethod
    def from_config(cls, config: dict) -> PluginSettings:
        return cls(**config)
```

Then, validator functions could be created (for internal use unless someone wants to extend the plugin api).

For example:
- Check if the plugin is enabled.
- Ensure that only one instance of the plugin is loaded if `single_instance` is true.
