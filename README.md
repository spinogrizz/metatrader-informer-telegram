## MT4Informer for MetaTrader 4

This is MT4Informer, a tool that keeps you updated about your MT4 trading bot. It sends you Telegram messages when specific stuff happens on your trading account. It helps you keep an eye on your trades but can't be at your computer all the time.

### What it does?

MT4Informer keeps an eye on your trades and lets you know when:

1. Your robot has taken a certain number of orders.
2. Your balance goes up by a certain amount (your bot's made some money!).
3. Your open profit/loss goes over a certain amount.

When any of this happens, MT4Informer will hit you up on Telegram with a message using [@RoboInformerBot](https://t.me/RoboInformerBot) (or your own bot if you compile it youself).


### Installation

Follow these steps to get MT4Informer running:

1. Download the `MT4Informer.mq4` file.
2. Open your MT4 terminal, click `File` > `Open Data Folder`.
3. Go to `MQL4/Experts`and drop the `MT4Informer.mq4` file in the that folder.
5. Restart your MT4 terminal. You should see MT4Informer in the `Navigator` pane under `Expert Advisors`. If it's not there, right-click in the `Navigator` pane and hit `Refresh`.
6. Create a new chart in your MT4 terminal and attach (drag) the MT4Informer EA to it. 
7. Add https://api.telegram.org to allowed addresses for WebRequest in the settings of your MT4 terminal.


### Settings

Here are the settings you can tweak:

| Setting                | Description                                                |
| ---------------------- | ---------------------------------------------------------- |
| `chat_id`              | The Telegram chat_id where you'll get the notifications    |
| `orders_warning`       | The number of orders at which you get a notification       |
| `profit_notify`        | The balance increase (in USD) that triggers a notification |
| `balance_down_warning` | The negative open profit/loss that triggers a notification |
| `is_usd_cent`          | Whether your account is in USD cent or USD                 |
| `nickname`             | Nickname of your advisor/terminal                 |


### How to attach it to your Telegram

If you want to use my Telegram bot to get informed:

1. Send **/start** to [@RoboInformerBot](https://t.me/RoboInformerBot).
2. Get your **chat_id** from  [@myidbot](https://t.me/myidbot).
3. Enter your chat_id to the settings pane of the informer/expert.
