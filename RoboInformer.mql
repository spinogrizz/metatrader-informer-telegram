//+------------------------------------------------------------------+
//|                                                     RoboInformer |
//|                                                    Denis Gryzlov |
//|                                                                  |
//+------------------------------------------------------------------+
#property copyright "Denis Gryzlov"
#property link      "https://github.com/spinogrizz/metatrader-informer-telegram"
#property version   "1.05"
#property strict


// Telegram Bot Token
string bot_token = "...";

// User variables
input string chat_id = "";                   // Your personal Telegram chat_id
input int orders_warning = 10;               // Limit when notify about too many orders taken
input int profit_notify = 10;                // Notify when balance jumps that many $
input double balance_down_warning = 500;     // An amount of negative USD on an open P/L to notify about
input bool is_usd_cent = true;               // Whether account is USD cent or USD
input string nickname = "";                  // Nickname of your advisor/terminal

// Flags to remember which notifications where already sent
bool event_orders_sent = false;
bool event_balance_sent = false;

// Remember balance on the previous timer tick
double last_balance = 0.0;

int OnInit() {
   EventSetTimer(30);
   last_balance = AccountInfoDouble(ACCOUNT_BALANCE);
   
   return(INIT_SUCCEEDED);
}

void OnDeinit(const int reason) {
   EventKillTimer();
}

void OnTimer() {
    double current_balance = AccountInfoDouble(ACCOUNT_BALANCE);

    string resultMessage = "";
    
    // Use multiplier for a USD cent accounts
    int usd = (is_usd_cent ? 100 : 1);

    // Event 1: Too many orders taken by a robot
    if (OrdersTotal() >= orders_warning && !event_orders_sent) {
        resultMessage = StringFormat("\\uD83D\\uDCE5 %d orders taken", OrdersTotal(), " orders taken");
        event_orders_sent = true;
    } else if (event_orders_sent && OrdersTotal() < orders_warning/2) {
        event_orders_sent = false;
    }

    // Event 2: Robot takes some profit by closing multiple pending orders 
    double profit = (current_balance - last_balance)/usd;
    if (profit >= profit_notify) {  
        resultMessage = StringFormat("\\uD83D\\uDCB0 +$%.0f", profit);
    } 

    // Event 3: Current open profit/loss is increasing above threshold
    double ordersSumProfit = AccountInfoDouble(ACCOUNT_PROFIT)/usd;
    
    if (ordersSumProfit < -balance_down_warning && !event_balance_sent) {
        resultMessage = StringFormat("\\uD83D\\uDCC9 Balance down -$%.0f", MathAbs(ordersSumProfit));
        event_balance_sent = true;
    } else if (event_balance_sent && ordersSumProfit > -balance_down_warning/2) { 
        event_balance_sent = false;
    }
    
    // Send notification 
    if ( resultMessage != "" ) {
        sendTelegramMessage(resultMessage);
    } 

    last_balance = current_balance;
}


void sendTelegramMessage(string text) {
   if ( nickname != "" ) {
      text = StringFormat("%s (%s)", text, nickname);
   }

   string url = StringFormat("https://api.telegram.org/bot%s/sendMessage", bot_token); 
   string data = StringFormat("{\"chat_id\": \"%s\", \"text\": \"%s\"}", chat_id, text);

   char post[], result[];
   StringToCharArray(data, post);

   char response[];
   string responseHeaders;
 
   int res = WebRequest("POST", url, "Content-Type: application/json", 10000, post, response, responseHeaders);
   
   if ( res == -1 ) {
      Print("WebRequest failed. Error code: ", GetLastError());
   }
}

