# Cldr Currencies

Packages the currency definitions from [CLDR](http://cldr.unicode.org) into a set of functions
to return currency data.

## Installation

The package can be installed by adding `ex_cldr_currencies` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:ex_cldr_currencies, "~> 2.5"}
  ]
end
```

## Using private use currencies

ISO4217 permits the creation of private use currencies. These are denoted by currencies that start with "X" followed by two characters.  New currencies can be created with `Cldr.Currency.new/2` however in order to do so a supervisor must be started which maintains and `:ets` table that holds the custom currencies.

### Starting the private use currency supervisor

The simplest way to start the private use currency supervisor is:
```elixir
iex> Cldr.Currency.start_link()
```

The preferred method however is to add the supervisor to your applications supervision tree. In your application module (ie the one that includes `use Application`):
```elixir
defmodule MyApp do
  use Application

  def start(_type, _args) do

    # Start the service which maintains the
    # :ets table that holds the private use currencies
    children = [
      Cldr.Currency
      ...
    ]

    opts = [strategy: :one_for_one, name: MoneyTest.Supervisor]
    Supervisor.start_link(children, opts)
  end
end
```

