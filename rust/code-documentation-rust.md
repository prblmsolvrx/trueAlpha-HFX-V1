# TrueAlpha-HFX-v1 Trading Strategy

## Overview

The TrueAlpha-HFX-v1 is a complex trading strategy implemented in Rust. It combines multiple technical indicators to generate buy and sell signals for financial markets. This strategy is based on a TradingView Pine Script and has been adapted for use in Rust applications.

## Components

1. Relative Strength Index (RSI)
2. Exponential Moving Average (EMA)
3. Bollinger Bands
4. (Optional) Directional Movement Index (DMI)
5. (Optional) Stochastic Oscillator

## Strategy Parameters

- `show_last`: Number of recent signals to display (1 or 2)
- `use_arrows`: Boolean flag to enable/disable arrow indicators
- `use_dmi_stochastic`: Boolean flag to enable/disable DMI-Stochastic signal

## Indicator Settings

- RSI: 14-period
- EMA: 200-period
- Bollinger Bands: 18, 20, and 25-period with 2 standard deviations

## Signal Generation

The strategy generates signals based on a complex set of conditions involving the interplay between various technical indicators. The exact logic for signal generation should be implemented in the `generate_signal` method of the `TrueAlphaHFXV1` struct.

## Usage

To use this strategy in your Rust application:

1. Initialize the `TrueAlphaHFXV1` struct with desired parameters.
2. Feed price data (close, high, low) into the `update` method for each new candle.
3. Check the returned `Option<Signal>` for buy or sell signals.

## Example

```rust
let mut strategy = TrueAlphaHFXV1::new(1, true, true);
let close_price = 100.0;
let high_price = 101.0;
let low_price = 99.0;

if let Some(signal) = strategy.update(close_price, high_price, low_price) {
    match signal {
        Signal::Buy => println!("Buy signal at price: {}", close_price),
        Signal::Sell => println!("Sell signal at price: {}", close_price),
    }
}
```

## Disclaimer

This trading strategy is for educational purposes only. Always perform your own analysis and risk assessment before using any trading strategy in live markets.