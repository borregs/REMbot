# REMbot
currently @testing stage. REMbot is a fork of zenbot 4.1.0 that utilizes old urlStringParser inside a docker container




## Docker-compose required
# runing:

docker-compose up -d (DAEMON)

docker-compose exec server zenbot list-selectors


# Usage

Usage: trade [options] [selector]

  run trading bot against live market data

  Options:

    --conf <path>                     path to optional conf overrides file
    --strategy <name>                 strategy to use
    --order_type <type>               order type to use (maker/taker)
    --paper                           use paper trading mode (no real trades will take place)
    --manual                          watch price and account balance, but do not perform trades automatically
    --non_interactive                 disable keyboard inputs to the bot
    --currency_capital <amount>       for paper trading, amount of start capital in currency
    --asset_capital <amount>          for paper trading, amount of start capital in asset
    --avg_slippage_pct <pct>          avg. amount of slippage to apply to paper trades
    --buy_pct <pct>                   buy with this % of currency balance
    --deposit <amt>                   absolute initial capital (in currency) at the bots disposal (previously --buy_max_amt)
    --sell_pct <pct>                  sell with this % of asset balance
    --markdown_buy_pct <pct>          % to mark down buy price
    --markup_sell_pct <pct>           % to mark up sell price
    --order_adjust_time <ms>          adjust bid/ask on this interval to keep orders competitive
    --order_poll_time <ms>            poll order status on this interval
    --sell_stop_pct <pct>             sell if price drops below this % of bought price
    --buy_stop_pct <pct>              buy if price surges above this % of sold price
    --profit_stop_enable_pct <pct>    enable trailing sell stop when reaching this % profit
    --profit_stop_pct <pct>           maintain a trailing stop this % below the high-water mark of profit
    --max_sell_loss_pct <pct>         avoid selling at a loss pct under this float
    --max_buy_loss_pct <pct>          avoid buying at a loss pct over this float
    --max_slippage_pct <pct>          avoid selling at a slippage pct above this float
    --rsi_periods <periods>           number of periods to calculate RSI at
    --poll_trades <ms>                poll new trades at this interval in ms
    --currency_increment <amount>     Currency increment, if different than the asset increment
    --keep_lookback_periods <amount>  Keep this many lookback periods max.
    --exact_buy_orders                instead of only adjusting maker buy when the price goes up, adjust it if price has changed at all
    --exact_sell_orders               instead of only adjusting maker sell when the price goes down, adjust it if price has changed at all
    --use_prev_trades                 load and use previous trades for stop-order triggers and loss protection
    --min_prev_trades                 minimum number of previous trades to load if use_prev_trades is enabled, set to 0 to disable and use trade time instead
    --disable_stats                   disable printing order stats
    --reset_profit                    start new profit calculation from 0
    --use_fee_asset                   Using separated asset to pay for fees. Such as binance's BNB or Huobi's HT
    --run_for <minutes>               Execute for a period of minutes then exit with status 0 (default: null)
    --debug                           output detailed debug info
    -h, --help                        output usage information

### Community

Join the Zenbot community on [Reddit](https://reddit.com/r/zenbot)!

## Disclaimer

- Zenbot is NOT a sure-fire profit machine. Use it AT YOUR OWN RISK.
- Crypto-currency is still an experiment, and therefore so is Zenbot. Meaning, both may fail at any time.
- Running a bot, and trading in general requires careful study of the risks and parameters involved. A wrong setting can cause you a major loss.
- Never leave the bot un-monitored for long periods of time. Zenbot doesn't know when to stop, so be prepared to stop it if too much loss occurs.
- Often times the default trade parameters will underperform vs. a buy-hold strategy, so run some simulations and find the optimal parameters for your chosen exchange/pair before going "all-in".

## Quick-start

### Step 1) Requirements

- Windows / Linux / macOS 10 (or Docker)
- [Node.js](https://nodejs.org/) (version 8.3.0 or higher) and [MongoDB](https://www.mongodb.com/).

### Step 2) Install zenbot 4

Run in your console,

```
git clone https://github.com/deviavir/zenbot.git
```

Or, without git,

```
wget https://github.com/deviavir/zenbot/archive/master.tar.gz
tar -xf zenbot-master.tar.gz
mv zenbot-master zenbot
```

Create your configuration file by copying `conf-sample.js` to `conf.js`:

```
cp conf-sample.js conf.js
```

- View and edit `conf.js`.
- It's possible to use zenbot in "paper trading" mode without making any changes.
- You must add your exchange API keys to enable real trading however.
- API keys do NOT need deposit/withdrawal permissions.


### Docker 

To run Zenbot under Docker, install Docker, Docker Compose, Docker Machine (if necessary) You can follow instructions at https://docs.docker.com/compose/install/

After installing (step 2 above),

```
cd zenbot
docker-compose up (-d if you don't want to see the log)
```

If you are running windows use the following command

```
docker-compose --file=docker-compose-windows.yml up
```

If you wish to run commands (e.g. backfills, list-selectors), you can run this separate command after a successful `docker-compose up -d`:

```
docker-compose exec server zenbot list-selectors
docker-compose exec server zenbot backfill <selector> --days <days>
```

#### Updating docker

In case you are behind on updates, you can run:
```
docker pull deviavir/zenbot:unstable
```
And re-run `docker-compose up -d` to start the new image.

`deviavir/zenbot` is automatically updated after every merge.
You can follow the tags/builds here: https://hub.docker.com/r/deviavir/zenbot/builds/

## Selectors

A "selector" is a short identifier that tells Zenbot which exchange and currency pair to act on. Use the form `{exchange_slug}.{asset}-{currency}`. A complete list of selectors your Zenbot install supports can be found with:

```
zenbot list-selectors

gdax:
  gdax.BTC-EUR   (BTC/EUR)
  gdax.BTC-GBP   (BTC/GBP)
  gdax.BTC-USD   (BTC/USD)
  gdax.ETH-BTC   (ETH/BTC)
  gdax.ETH-USD   (ETH/USD)
  gdax.LTC-BTC   (LTC/BTC)
  gdax.LTC-USD   (LTC/USD)

poloniex:
  poloniex.AMP-BTC   (Synereo AMP/BTC)
  poloniex.ARDR-BTC   (Ardor/BTC)
  poloniex.BCN-BTC   (Bytecoin/BTC)
  poloniex.BCN-XMR   (Bytecoin/XMR)
  poloniex.BCY-BTC   (BitCrystals/BTC)

...etc
```

## Run a simulation for your selector

To backfill data (provided that your chosen exchange supports it), use:

```
zenbot backfill <selector> --days <days>
```

You can also select start and end date:

```
zenbot backfill <selector> --start="YYYYMMDDhhmm" --end="YYYYMMDDhhmm"
```
Note you can use them separately.

After you've backfilled, you can run a simulation:

```
zenbot sim <selector> [options]
```

For a list of options for the `sim` command, use:

```
zenbot sim --help

```

For additional options related to the strategy, use:

```
zenbot list-strategies
```

- By default the sim will start with 1000 units of currency. Override with `--currency_capital` and `--asset_capital`.
- Open `sim_result.html` in your browser to see a candlestick graph with trades.

### Screenshot and example result

Zenbot outputs an HTML graph of each simulation result. In the screenshot below, the pink arrows represent the bot buying (up arrow) and selling (down arrow) as it iterated the historical data of [GDAX](https://gdax.com/) exchange's BTC/USD product.

![screenshot](https://cloud.githubusercontent.com/assets/106763/25983930/7e5f9436-369c-11e7-971b-ba2916442eea.png)

```
end balance 2954.50 (195.45%)
buy hold 1834.44 (83.44%)
vs. buy hold 61.06%
110 trades over 91 days (avg 1.21 trades/day)
```

Zenbot started with $1,000 USD and ended with $2,954.50 after 90 days, making 195% ROI! In spite of a buy/hold strategy returning a respectable 83.44%, Zenbot has considerable potential for beating buy/holders.

- Note that this example used tweaked settings to achieve optimal return: `--profit_stop_enable_pct=10`, `--profit_stop_pct=4`, `--trend_ema=36`, and `--sell_rate=-0.006`. Default parameters yielded around 65% ROI.
- [Raw data](https://gist.github.com/carlos8f/b09a734cf626ffb9bb3bcb1ca35f3db4) from simulation

## Running zenbot

The following command will launch the bot, and if you haven't touched `c.selector` in `conf.js`, will trade the default BTC/USD pair on GDAX.

```
zenbot trade [--paper] [--manual]
```

Use the `--paper` flag to only perform simulated trades while watching the market.

Use the `--manual` flag to watch the price and account balance, but do not perform trades automatically.

Here's how to run a different selector (example: ETH-BTC on Poloniex):

```
zenbot trade poloniex.eth-btc
```

For a full list of options for the `trade` command, use:

```
zenbot trade --help

  Usage: trade [options] [selector]

  run trading bot against live market data

## Interactive controls

While the `trade` command is running, Zenbot will respond to these keypress commands:

- Pressing `b` will trigger a buy, `s` for sell, and `B` and `S` for market (taker) orders.
- Pressing `c` or `C` will cancel any active orders.
- Pressing `m` or `M` will toggle manual mode (`--manual`)

These commands can be used to override what the bot is doing. Or, while running with the `--manual` flag, this allows you to make all the trade decisions yourself.

### noop strategy

If you want to use the bot without it trading for you, but just use it for the balance overview and manual trades, you can start the bot with `--strategy noop`, the bot will not trade automatically.

## Conf/argument override files

To run `trade` or `sim` commands with a pre-defined set of options, use:

```
zenbot trade --conf <path>
```

Where `<path>` points to a JS file that exports an object hash that overrides any conf or argument variables. For example, this file will run gdax.ETH-USD with options specific for that market:

```
var c = module.exports = {}

// ETH settings (note: this is just an example, not necessarily recommended)
c.selector = 'gdax.ETH-USD'
c.period = '10m'
c.trend_ema = 20
c.neutral_rate = 0.1
c.oversold_rsi_periods = 20
c.max_slippage_pct = 10
c.order_adjust_time = 10000
```

## GUI

A basic web UI is available at the url stated during startup.  This port can be configured in the conf.js or randomly assigned.
In it's infancy, there are a few caveats with the current UI.
- In order to have statistics displayed, they must first be dumped from the CLI.  Pressing `D` will refresh the statistics on each refresh of the dashboard.
- Currently the data is mostly static with the exception of the tradingview charts.
- Currently only READ-ONLY

## Reading the console output

![console](https://rawgit.com/deviavir/zenbot/master/assets/console.png)

From left to right:

- Timestamp in local time (grey, blue when showing "live" stats)
- Asset price in currency (yellow)
- Percent change of price since last period (red/green)
- Volume in asset since last period (grey)
- [RSI](http://stockcharts.com/school/doku.php?id=chart_school:technical_indicators:relative_strength_index_rsi) ANSI graph (red/green)
- `trend_ema_rate` (red/green, explained below)
- Current signal or action, including `buy`, `sell`, `buying`, `selling`, `bought`, `sold` and `last_trade_worth` (percent change in the trend direction since last buy/sell)
- Account balance (asset)
- Account balance (currency)
- Profit or loss percent (can be reset with `--reset_profit`)
- Gain or loss vs. buy/hold strategy

## Strategies

### The `trend_ema` strategy (default)

- The default strategy is called `trend_ema` and resides at `./extensions/strategies/trend_ema`.
- Defaults to using a 2m period, but you can override this with adding e.g. `--period=5m` to the `sim` or `trade` commands.
- Computes the 26-period EMA of the current price, and calculates the percent change from the last period's EMA to get the `trend_ema_rate`
- Considers `trend_ema_rate >= 0` an upwards trend and `trend_ema_rate < 0` a downwards trend
- Filters out low values (whipsaws) by `neutral_rate`, which when set to `auto`, uses the standard deviation of the `trend_ema_rate` as a variable noise filter.
- Buys at the beginning of upwards trend, sells at the beginning of downwards trend
- If `oversold_rsi` is set, tries to buy when the RSI dips below that value, and then starts to recover (a counterpart to `--profit_stop_enable_pct`, which sells when a percent of profit is reached, and then dips)
- The bot will always try to avoid trade fees, by using post-only orders and thus being a market "maker" instead of a "taker". Some exchanges will, however, not offer maker discounts.

### The `macd` strategy

The moving average convergence divergence calculation is a lagging indicator, used to follow trends.

- Can be very effective for trading periods of 1h, with a shorter period like 15m it seems too erratic and the Moving Averages are kind of lost.
- It's not firing multiple 'buy' or 'sold' signals, only one per trend, which seems to lead to a better quality trading scheme.
- Especially when the bot will enter in the middle of a trend, it avoids buying unless it's the beginning of the trend.

### The `rsi` strategy

Attempts to buy low and sell high by tracking RSI high-water readings.

- Effective in sideways markets or markets that tend to recover after price drops.
- Risky to use in bear markets, since the algorithm depends on price recovery.
- If the other strategies are losing you money, this strategy may perform better, since it basically "reverses the signals" and anticipates a reversal instead of expecting the trend to continue.

### The `sar` strategy

Uses a [Parabolic SAR](http://stockcharts.com/school/doku.php?id=chart_school:technical_indicators:parabolic_sar) indicator to trade when SAR trend reverses.

- Tends to generate earlier signals than EMA-based strategies, resulting in better capture of highs and lows, and better protection against quick price drops.
- Does not perform well in sideways (non-trending) markets, generating more whipsaws than EMA-based strategies.
- Most effective with short period (default is 2m), which means it generates 50-100 trades/day, so only usable on GDAX (with 0% maker fee) at the moment.
- Tested live, [results here](https://github.com/carlos8f/zenbot/pull/246#issuecomment-307528347)

### The `speed` strategy

Trade when % change from last two 1m periods is higher than average.

**This strategy is experimental and has WILDLY varying sim results. NOT RECOMMENDED YET.**

- Like the sar strategy, this generates early signals and can be effective in volatile markets and for sudden price drop protection.
- Its weakness is that it performs very poorly in low-volatility situations and misses signals from gradually developing trends.

### Tips for tweaking options

- Trade frequency is adjusted with a combination of `--period` and `--trend_ema`. For example, if you want more frequent trading, try `--period=5m` or `--trend_ema=15` or both. If you get too many ping-pong trades or losses from fees, try increasing `period` or `trend_ema` or increasing `neutral_rate`.
- Sometimes it's tempting to tell the bot trade very often. Try to resist this urge, and go for quality over quantity, since each trade comes with a decent amount of slippage and whipsaw risk.
- `--oversold_rsi=<rsi>` will try to buy when the price dives. This is one of the ways to get profit above buy/hold, but setting it too high might result in a loss if the price continues to fall.
- In a market with predictable price surges and corrections, `--profit_stop_enable_pct=10` will try to sell when the last buy hits 10% profit and then drops to 9% (the drop % is set with `--profit_stop_pct`). However in strong, long uptrends this option may end up causing a sell too early.
- For Kraken and GDAX you may wish to use `--order_type="taker"`, this uses market orders instead of limit orders. You usually pay a higher fee, but you can be sure that your order is filled instantly. This means that the sim will more closely match your live trading. Please note that GDAX does not charge maker fees (limit orders), so you will need to choose between not paying fees and running the risk orders do not get filled on time, or paying somewhat high % of fees and making sure your orders are always filled on time.

## Notifiers

Zenbot employs various notifiers to keep you up to date on the bot's actions. We currently send a notification on a buy and on a sell signal.

### pushbullet

Supply zenbot with your api key and device ID and we will send your notifications to your device.
https://www.pushbullet.com/

### Slack

Supply zenbot with a webhook URI and zenbot will push notifications to your webhook.
https://slack.com/

### XMPP

Supply zenbot with your XMPP credentials and zenbot will send notifications by connecting to your XMPP, sending the notification, and disconnecting.
https://xmpp.org/

### IFTTT

Supply zenbot with your IFTTT maker key and zenbot will push notifications to your IFTTT.
https://ifttt.com/maker_webhooks

### DISCORD

Supply zenbot with your Discord webhook id and webhook token zenbot will push notifications to your Discord channel.

How to add a webhook to a Discord channel
https://support.discordapp.com/hc/en-us/articles/228383668

### Prowl

Supply zenbot with your Prowl API key and zenbot will push notifications to your Prowl enabled devices.
https://www.prowlapp.com/

### TextBelt

Supply zenbot with your TextBelt API key and zenbot will send SMS notifications to your cell phone.
https://www.textbelt.com/

## Rest API

You can enable a Rest API for Zenbot by enabling the following configuration
```
c.output.api = {}
c.output.api.on = true
c.output.api.port = 0 // 0 = random port
```
You can choose a port, or pick 0 for a random port.

Once you did that, you can call the API on: http://\<hostname\>:\<port\>/trades

## Manual trade tools

Zenbot's order execution engine can also be used for manual trades. Benefits include:

- Avoids market-order fees by using a short-term limit order
- Can automatically determine order size from account balance
- Adjusts order every 30s (if needed) to ensure quick execution
- If an order is partially filled, attempts to re-order with remaining size

The command to buy is:

```
zenbot buy <selector> [--size=<size>] [--pct=<pct>]
```

For example, to use your remaining USD balance in GDAX to buy Bitcoin:

```
zenbot buy gdax.BTC-USD
```

Or to sell 10% of your BTC,

```
zenbot sell gdax.BTC-USD --pct=10
```
### License: MIT

- Copyright (C) 2018 Carlos Rodriguez
- Copyright (C) 2018 Terra Eclipse, Inc. (http://www.terraeclipse.com/)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the &quot;Software&quot;), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is furnished
to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED &quot;AS IS&quot;, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
