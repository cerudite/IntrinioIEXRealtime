#Example Usage:

```package org.cerudite.testing;

import org.cerudite.intrinio.IEXRealtime;
import org.cerudite.intrinio.IEXRealtime.TextMessageHandler;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;


public class IntrinioTesting
{

  private static final String STOCKS_CSV = "MSFT,AAPL,GE";

  public static void main(String[] args)
  {
    JSONParser parser = new JSONParser();

    IEXRealtime realtime = new IEXRealtime(
            "", // username
            "", // password
            false, // debug
            new TextMessageHandler()  // Message Handler interface Callback
    {
      @Override
      public void handleMessage(String msg) // handleMessage override
      {
        try {
          JSONObject json = (JSONObject) parser.parse(msg);
          String event = (String) json.get("event");
          switch (event) {
            case "quote":
              System.out.println("QUOTE: " + json.toJSONString());
            default:
              IEXRealtime.debug("Non-quote message:" + json.toJSONString());
          }
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    });

    if (STOCKS_CSV != null) {
      String[] stocksArray = STOCKS_CSV.split(",");
      realtime.join(stocksArray);
    }
  }

}```
