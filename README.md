# Possible bug demo

## Summary

Using Elixir `1.11.0-rc.0`, when called from `config/runtime.exs`, `Config.config/3` does not deep merge keyword lists of options with those set in the other config files; rather, it overwrites them.

## Demo

- Use the Elixir and Erlang versions in `.tool-versions`
- `iex -S mix` and `Application.get_env(:config_demo, :foo)`. Observe that only `[d: :d]`, set in `config/runtime.exs`, is returned.
- Comment out the `config` call in `config/runtime.exs`
- `iex -S mix` and `Application.get_env(:config_demo, :foo)`. Observe that `[a: :a, b: :b2, c: :c]`, the merged value of `config/config.exs` and `config/runtime.exs`, is returned.

## Thoughts

Possibly this is because `Config.get_config!/0` and `put_config!/1` use the process dictionary, and `config/runtime.exs` is executed in a different process?
