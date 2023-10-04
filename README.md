# Combined EMA Crossover and Crossunder Strategy

This is a simple trading strategy script written in Pine Script language for use in TradingView. The strategy combines two Exponential Moving Averages (EMAs) to generate buy and sell signals based on crossover and crossunder events.

## Strategy Overview

The strategy involves two EMAs, a short-term EMA, and a long-term EMA. When the short-term EMA crosses above the long-term EMA, it generates a "Buy" signal, and when it crosses below the long-term EMA, it generates a "Short" signal. Additionally, there is a condition to exit any active "Buy" or "Short" positions when a 9-period EMA crosses over a 13-period EMA.

## Strategy Code

```pinescript
//@version=4
strategy("Combined EMA Crossover and Crossunder Strategy", shorttitle="Combined EMA Cross", overlay=true)

// Input parameters
emaLengthShort = input(50, title="Short EMA Length")
emaLengthLong = input(100, title="Long EMA Length")

// Calculate EMAs
emaShort = ema(close, emaLengthShort)
emaLong = ema(close, emaLengthLong)

// Plot EMAs on the chart
plot(emaShort, color=color.blue, title="Short EMA")
plot(emaLong, color=color.red, title="Long EMA")

// Create strategy conditions
emaCrossOver = crossover(emaShort, emaLong)
emaCrossUnder = crossunder(emaShort, emaLong)

// Buy and exit conditions
strategy.entry("Buy", strategy.long, when=emaCrossOver)
strategy.close("Buy", when=emaCrossUnder)

// New exit condition: 9-period EMA crosses under 13-period EMA
ema9 = ema(close, 9)
ema13 = ema(close, 13)
emaCrossUnder9_13 = crossunder(ema9, ema13)

strategy.close("Buy", when=emaCrossUnder9_13)

// Short and exit short conditions
strategy.entry("Short", strategy.short, when=emaCrossUnder)
strategy.close("Short", when=emaCrossOver)

// Exit short position when 9 EMA crosses over 13 EMA
emaCrossOver9_13 = crossover(ema9, ema13)
strategy.close("Short", when=emaCrossOver9_13)
```

## How to Use

1. Copy the provided Pine Script code.
2. Open TradingView and create a new Pine Script strategy.
3. Paste the code into the Pine Script editor.
4. Adjust the input parameters (`emaLengthShort` and `emaLengthLong`) to your preferred values.
5. Apply the strategy to your chart and backtest it to evaluate its performance.

This strategy uses EMAs to generate trading signals, and it includes conditions for entry and exit. However, please note that this is a simple example and should not be considered financial advice. Always perform thorough testing and analysis before using any trading strategy in a live market.
