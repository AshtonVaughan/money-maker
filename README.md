<div align="center">

# money-maker

**LSTM-powered EUR/USD scalping signal engine for MetaTrader 5**

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![MetaTrader](https://img.shields.io/badge/MetaTrader-5-2962FF?style=flat-square)](https://www.metatrader5.com/)
[![License](https://img.shields.io/badge/license-MIT-94A3B8?style=flat-square)](.)

*Real-time LSTM model that predicts TP/SL hit probability on EUR/USD. Forward testing only - no live execution.*

</div>

<br>

## How It Works

Tick data is fetched from MetaTrader 5 every 3 seconds, processed into fixed-length sequences, and passed through a trained LSTM network. The model outputs two probabilities: the likelihood of hitting the 2-pip Take Profit first vs the 1.5-pip Stop Loss first. All signals are logged - no orders are placed.

```
MT5 Tick Feed (every 3s)
        |
        v
  Feature Pipeline       <- normalization, sequence windowing
        |
        v
    LSTM Model            <- sequence encoder + dense output head
        |
        v
  Signal Logger           <- timestamped TP/SL probabilities, no execution
```

| Parameter     | Value              |
|---------------|--------------------|
| Symbol        | EUR/USD            |
| Tick interval | 3 seconds          |
| TP target     | 2 pips             |
| SL target     | 1.5 pips           |
| Model         | LSTM (multi-layer) |
| Mode          | Forward testing    |

<br>

## Quick Start

### 1. Install

```bash
git clone https://github.com/AshtonVaughan/money-maker.git
cd money-maker && pip install -r requirements.txt
```

### 2. Configure (optional)

Create a `.env` file to connect to a specific account:

```env
MT5_LOGIN=123456
MT5_PASSWORD=your_password
MT5_SERVER=YourBroker-Demo
```

If no `.env` is present, the system uses the currently active MT5 terminal session.

### 3. Train the model

```bash
python trainer.py
```

Downloads 3 months of historical EUR/USD data from MT5, builds sequences, trains the LSTM, and saves weights to `models/`.

### 4. Run live predictions

```bash
python main.py
```

Polls tick data every 3 seconds. Predictions are written to log with timestamps.

<br>

## Requirements

- MetaTrader 5 terminal (installed and running)
- MT5 account (demo or live)
- Python 3.8+

<br>

## Disclaimer

This project is experimental and for educational purposes only. It does not constitute financial advice and is not intended for live trading with real capital. The author accepts no liability for financial losses arising from use of this software.

<br>

## License

MIT
