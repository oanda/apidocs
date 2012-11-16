# News Endpoints

| Endpoint | Description |
| ---- | ---- |
| GET /news | Get a list of news items |
| GET /news/:article_id | Get the body of a news item |

## GET /accounts

#### Request
    http://api.oanda.com/v1/news

#### Response
<pre>
    {
      "ReturnCode":0,
      "ActionCode":1,
      "Result":true,
      "MaxSeqNum":854191223,
      "NewsList": [ {"Uid":10413,
                     "Time":1332526860,
                     "Source":"Thomson Financial",
                     "SourceId":"252255",
                     "Language":"ENG",
                     "Author":"",
                     "Headline":"Daily High/Lows 1  ",
                     "Preview":"<table> <tr> <th>Rate</th><th>Today 18:16</th><th>Thu 22 Mar</th><th>Wed 21 Mar</th><th>Tue 20 Mar</th><th>Mon 19 Mar</th> </tr> <tr> <th>EUR/USD</th><td>1.3190/1.3294</td><td>1.3134/1.3255</td><td>1.3178/1.3286</td><td>1.3172/1.3253</td><td>1.3142/1.3265</td> </tr> <tr class=\"altrow\"> <th>USD/JPY</th></tr></table>",
                     "RelatedLinks":[],
                     "Images":[],
                     "Code":["2371"],
                     "Body":""},
                    {"Uid":831641,
                     "Time":1332526500,
                     "Source":"Dow Jones",
                     "SourceId":"DN20120323010700",
                     "Language":"ENG",
                     "Author":"",
                     "Headline":"  WSJ BLOG/MarketBeat: Spain's Critical Issue Is Its Private Debt",
                     "Preview":"(This story has been posted on The Wall Street Journal Online's Market Beat blog at <a href=\"http://blogs.wsj.com/marketbeat\">http://blogs.wsj.com/marketbeat</a>",
                     "RelatedLinks":[],
                     "Images":[],
                     "Code":["DJ_HOT","DJ_BBVA","DJ_BBVA.MC","DJ_FCC.MC","DJ_SAN.MC","DJ_STD","DJ_ES0113211835","DJ_ES0113900J37","DJ_ES0122060314","DJ_US05946K1016","DJ_US05964H1059","DJ_I/BAN","DJ_I/BNK","DJ_I/CON","DJ_I/XBAT","DJ_I/XDJGI","DJ_I/XES","DJ_I/XGDW","DJ_I/XGTI","DJ_I/XIBEX","DJ_I/XNYA","DJ_I/XSLI","DJ_I/XST5","DJ_N/BKG","DJ_N/CMDI","DJ_N/CMR","DJ_N/DJCB","DJ_N/DJCS","DJ_N/DJEP","DJ_N/DJFI","DJ_N/DJG","DJ_N/DJG7","DJ_N/DJGP","DJ_N/DJI","DJ_N/DJIB","DJ_N/DJIN","DJ_N/DJMB","DJ_N/DJN","DJ_N/DJOS","DJ_N/DJPF","DJ_N/DJQS","DJ_N/DJSN","DJ_N/DJWB","DJ_N/DN","DJ_N/ECR","DJ_N/EMR","DJ_N/EUCM","DJ_N/EWR","DJ_N/FXW","DJ_N/MADN","DJ_N/NACM","DJ_N/OSAG","DJ_N/OSCM","DJ_N/OSFF","DJ_N/OSFR","DJ_N/OSME","DJ_N/OSTR","DJ_N/UKMR","DJ_N/WED","DJ_N/WER","DJ_N/ADR","DJ_N/BLOG","DJ_N/CCS","DJ_N/CEA","DJ_N/CEM","DJ_N/CMEC","DJ_N/CNW","DJ_N/DJFN","DJ_N/DJS","DJ_N/DJSS","DJ_N/DJWI","DJ_N/ECO","DJ_N/EIE","DJ_N/EMI","DJ_N/EMJ","DJ_N/GENI","DJ_N/HIY","DJ_N/HOT","DJ_N/IEI","DJ_N/IEJ","DJ_N/IEN","DJ_N/JIE","DJ_N/SNEW","DJ_N/WEI","DJ_N/WLS","DJ_M/FCL","DJ_M/IDU","DJ_M/NND","DJ_M/USD","DJ_P/BKG","DJ_P/DRT","DJ_P/EIE","DJ_P/EWR","DJ_P/FXHT","DJ_P/MAAD","DJ_P/NIB","DJ_P/NIP","DJ_P/PSH","DJ_P/WMAI","DJ_P/WMMI","DJ_R/CN","DJ_R/EC","DJ_R/EU","DJ_R/EZN","DJ_R/NME","DJ_R/PO","DJ_R/SP","DJ_R/US","DJ_R/WEU","DJ_S/ALL","DJ_S/CALL","DJ_S/GNP","DJ_P"],
                     "Body":""}
                ]
    }
</pre>

#### Parameters
| Name | Description |
| ---- | ----------- |
| language | The language of the request news. Possible values: ARA, DEU, ENG, ZHO, RUS |
| maxCount | Maximum number of news item to return. Default: 50 |
| seqNum | TODO: clarify if it behaves like minId |

#### Required scope
read

## GET /accounts/:account_id
#### Request
    http://api.oanda.com/v1/news/:article_id

#### Response
    {
    "ReturnCode":0,"ActionCode":4,"Result":true,"Article":{"Uid":10427,"Time":1353074762,"Source":"Thomson Financial","Language":"ENG","Author":"","Headline":"EUR/USD Trader  ","Preview":"<div id=\"tablefield-wrapper-3711502-0\" class=\"tablefield-wrapper\"><table id=\"tablefield-3711502-0\" class=\"tablefield sticky-enabled\"> <thead><tr><th>Level</th><th>Technical Significance</th> </tr></thead> <tbody> <tr class=\"odd\"><td> 1.2900 </td><td> Psychological Level </td> </tr></tbody></table></div>","RelatedLinks":[],
    "Images":[],
    "Code":["2334"],
    "Body":"<html>
    <head><title>EUR/USD Trader  </title>
    <style>.article-body {white-space: pre-line;}</style>
    </head>
    <body>
    <div class=\"date\">Fri Nov 16 09:06:02 2012</div>
    <div class=\"article-body\">
    <div id=\"tablefield-wrapper-3711502-0\" class=\"tablefield-wrapper\"><table id=\"tablefield-3711502-0\" class=\"tablefield sticky-enabled\">
     <thead><tr><th>Level</th><th>Technical Significance</th> </tr></thead>
    <tbody>
     <tr class=\"odd\"><td> 1.2900 </td><td> Psychological Level      </td> </tr>
     <tr class=\"even\"><td> 1.2876 </td><td> Daily High Nov 7         </td> </tr>
     <tr class=\"odd\"><td> 1.2810 </td><td> 200-Day MA               </td> </tr>
     <tr class=\"even\"><td> 1.2802 </td><td> Daily High Nov 15        </td> </tr>
     <tr class=\"odd\"><td>1.2741</td><td>      14:00 GMT FRI 16 NOV</td> </tr>
     <tr class=\"even\"><td> 1.2701 </td><td> Daily Low Nov 14         </td> </tr>
     <tr class=\"odd\"><td> 1.2662 </td><td> Daily Low Nov 13         </td> </tr>
     <tr class=\"even\"><td> 1.2627 </td><td> Daily Low Sep 7          </td> </tr>
     <tr class=\"odd\"><td> 1.2607 </td><td> 50% of 1.2042 - 1.3173   </td> </tr>
    </tbody>
    </table>
    </div><div id=\"current_trade\"><table><tr><th>Position</th><th>At</th> <th>Open/Close</th> <th>Target</th> <th>Stop</th> </tr><tr><td>FLAT</td><td>1.2715</td><td>2012-11-13T11:39:00</td><td></td><td></td></tr></table></div><div id=\"commentary\" >Failed to clear the 200 DMA yday and prices have eased lower again. The 10 DMA is under threat intraday and deeper pullbacks could be on the cards as hrlies tick south. Day charts are mixed and trying to maintain a bullish bias but below the 200 DMA we will stick with the sell strength strategy. Re-evaluate above the line. (AH)</div>

    Copyright (c) 2012 Thomson Reuters - IFRMarkets
    </div>
    </body>
    </html>"}
    }

#### Required scope
read



## GET template
#### Request
#### Response
#### Parameters
#### Required scope
