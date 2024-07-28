





//@version=4
study(title="trueAlpha-hfx-v1", shorttitle="trueAlpha-hfx-v1", overlay=true)

showlast = input(title = "Show Last 1 or 2", type= input.integer, defval = 1, minval = 1, maxval = 2)


arr = input(defval = true, title="Arrow. Timeframe = 5 min. ")

ds = input(defval = true, title="DMI - Stochastic Signal")

res = "5"


wwma(l, p) =>
    wwma = 0.0
    wwma := (nz(wwma[1]) * (l - 1) + p) / l
    wwma

// Inputs
DMIlength = 10
Stolength = 3

// DMI Osc Calc
hiDiff = high - high[1]
loDiff = low[1] - low
plusDM = hiDiff > loDiff and hiDiff > 0 ? hiDiff : 0
minusDM = loDiff > hiDiff and loDiff > 0 ? loDiff : 0
ATR = wwma(DMIlength, tr)
PlusDI = 100 * wwma(DMIlength, plusDM) / ATR
MinusDI = 100 * wwma(DMIlength, minusDM) / ATR
osc = PlusDI - MinusDI

// DMI Stochastic Calc
hi = highest(osc, Stolength)
lo = lowest(osc, Stolength)

Stoch = sum(osc - lo, Stolength) / sum(hi - lo, Stolength) * 100


crossUp = Stoch[1] < 10 and Stoch > 10 ? 1 : 0
crossDown = Stoch[1] > 90 and Stoch < 90 ? 1 : 0

cu = security(syminfo.tickerid, res, crossUp)

cd = security(syminfo.tickerid, res, crossDown)

upDmi = Stoch >= 90
downDmi = Stoch <= 10


ad = security(syminfo.tickerid, res, upDmi)

bd = security(syminfo.tickerid, res, downDmi)

plotchar(ds ? cu : na, title="Buy", char='B', location=location.bottom, color=color.green, transp=0, offset=0, editable=false, show_last =100, size = size.auto)
plotchar(ds ? cd : na, title="Sell", char='S', offset=0, location=location.top, color=color.red, transp=0, editable = false, show_last = 100, size = size.auto)


//Binary
LRG_Channel_TF_mins_D_W_M = "30"
Range2 = 1

SELL = security(syminfo.tickerid, LRG_Channel_TF_mins_D_W_M, highest(Range2), lookahead=barmerge.lookahead_on)
BUY = security(syminfo.tickerid, LRG_Channel_TF_mins_D_W_M, lowest(Range2), lookahead=barmerge.lookahead_on)

HI = plot(SELL, color=SELL != SELL[1] ? na : #FF0000, linewidth=2)
LO = plot(BUY, color=BUY != BUY[1] ? na : #008000, linewidth=2)

Hcon = high >= SELL
Lcon = low <= BUY

//EMAS/

M1 = ema(close, 200)

colorema = M1 >=  close[1] ? #ff3b31 : M1 < close[1] ?  #00af9b : na

plot(M1, color = colorema, linewidth=4, transp=10, title="EMA ")


//BB1
shorttitle3 = "BB"
title3 = "Bollinger Bands"
overlay3 = true
BB = 18
src4 = close
mult = 2.0
basis = sma(src4, BB)
dev = mult * stdev(src4, BB)
upper = basis + dev
lower = basis - dev
plot(basis, color=color.new(#ff0000,0), linewidth=1, title="BB Median 18", display = display.none)
p1 = plot(upper, color=color.new(#ff3b31, 35), linewidth=3, title="BB High 18")
p2 = plot(lower, color=color.new(#00af9b, 35), linewidth=3, title="BB Low 18")
fill(p1, p2, title = "Background", color = color.new(#FFFFFF, 90))

//bb2
BB2 = 20
src5 = close
mult2 = 2.0
basis2 = sma(src5, BB2)
dev2 = mult2 * stdev(src5, BB2)
upper2 = basis2 + dev2
lower2 = basis2 - dev2
plot(basis2, color=color.new(#000000, 0), linewidth=3, title="BB Median 20")
p3 = plot(upper2, color=color.new(#ff3b31, 35), linewidth=3, title="BB High 20")
p4 = plot(lower2, color=color.new(#00af9b, 35), linewidth=3, title="BB Low 20")
fill(p3, p4, title = "Background", color = color.new(#FFFFFF, 90))



//bb3
BB3 = 25
src6 = close
mult3 = 2.0
basis3 = sma(src6, BB3)
dev3 = mult3 * stdev(src6, BB3)
upper3 = basis3 + dev3
lower3 = basis3 - dev3
plot(basis3, color=color.new(#ff9900, 0), linewidth=1, title="BB Median 25", display = display.none)
p5 = plot(upper3, color=color.new(#ff3b31, 35), linewidth=3, title="BB High 25")
p6 = plot(lower3, color=color.new(#00af9b, 35), linewidth=3, title="BB Low 25")
fill(p5, p6, title = "Background", color = color.new(#FFFFFF, 90))



//RSI
up = rma(max(change(open), 0), 1)
down = rma(-min(change(open), 0), 1)
rsi = down == 0 ? 100 : up == 0 ? 0 : 100 - 100 / (1 + up / down)


Put = rsi > 70

Call = rsi < 30



//EURUSD
a = security(syminfo.tickerid, res, Put)

b = security(syminfo.tickerid, res, Call)




//RSI 4
//RSI-------------------------------------------------------------------------|
len = 4 //                                                                    |
src = close //                                                                |
up1 = rma(max(change(src), 0), len)  //                                       |
down1 = rma(-min(change(src), 0), len) //                                     |
rsi1 = down1 == 0 ? 100 : up1 == 0 ? 0 : 100 - (100 / (1 + up1 / down1)) //   |
//                                                                            |
vrsi = rsi1 > 70 //                                                           |
//                                                                            |
crsi = rsi1 < 30 //                                                           | 
//                                                                            |
//                                                                            |
ar = security(syminfo.tickerid, res, vrsi)//                                  |
//                                                                            |
br= security(syminfo.tickerid, res, crsi)//                                   |
//----------------------------------------------------------------------------|



//Stoch 5/1/1
//Stoch------------------------------------------------------|
periodk = 5 //                                               |
smoothk = 1 //                                               |
periodd = 1 //                                               |
k = sma(stoch(close, high, low, periodk), smoothk) //        |
d = sma(k, periodd) //                                       |
//                                                           |
//                                                           |
vsd = d > 80 //                                              |
csd = d < 20 //                                              |
//                                                           |
//                                                           |
as = security(syminfo.tickerid, res, vsd)//                  |
//                                                           |
bs= security(syminfo.tickerid, res, csd)//                   |
//-----------------------------------------------------------|
//Stoch 

//BB
//Bbsigal------------------------------------------------|
mult10 = 2.0 //                                          |
basis10 = sma(close, 18) //                              |
dev10 = mult10 * stdev(close, 18) //                     |
upper10 = basis10 + dev10 //                             | 
lower10 = basis10 - dev10 //                             |
//                                                       |
h = high //                                              |
//                                                       |
l = low //                                               |
//                                                       |
//                                                       |
vbb = h >= upper10 //                                    |
cbb = l <= lower10 //                                    |
//                                                       |
//                                                       |
ab = security(syminfo.tickerid, res, vbb)//              |
//                                                       |
bb= security(syminfo.tickerid, res, cbb)//               |
//-------------------------------------------------------|
//BB



//                                      Primera alerta                       \\
//Condition Stoch 1--------------|
stoch_arrow_sell = a and as //   |
//                               |
stoch_arrow_buy = b and bs //    |
//-------------------------------|


//condition RSI 1----------------|
rsi_arrow_sell = a and ar //     |
//                               |
rsi_arrow_buy = b and br //      |
//-------------------------------|

//condition DMI 1----------------|
dmi_arrow_sell = a and ad //     |
//                               |
dmi_arrow_buy = b and bd //      |
//-------------------------------|

//condition bb  1----------------|
bb_arrow_sell = a and ab //      |
//                               |
bb_arrow_buy = b and bb //       |
//-------------------------------|

//condition S/R  1-----------------|
sr_arrow_sell = a and Hcon //      |
//                                 |
sr_arrow_buy = b and Lcon //       |
//---------------------------------|

//orone****************************************************************************************************|
two_arrow_sell = rsi_arrow_sell or dmi_arrow_sell or stoch_arrow_sell or bb_arrow_sell or sr_arrow_sell//  |
//                                                                                                         | 
two_arrow_buy = stoch_arrow_buy or rsi_arrow_buy or dmi_arrow_buy or bb_arrow_buy or sr_arrow_buy//        |    
//*********************************************************************************************************|    




//                                      Segunda alerta                       \\
//----------------Stoch--------------------|
//Condition Stoch and RSI 1----------------|
stoch_rsi_arrow_sell = a and as and ar//   |
//                                         |
stoch_rsi_arrow_buy = b and bs and br//    |
//-----------------------------------------|

//Condition Stoch and Dmi 1----------------|
stoch_dmi_arrow_sell = a and as and ad//   |
//                                         |
stoch_dmi_arrow_buy = b and bs and bd//    |
//-----------------------------------------|

//Condition Stoch and bb 1-----------------|
stoch_bb_arrow_sell = a and as and ab//    |
//                                         |
stoch_bb_arrow_buy = b and bs and bb//     |
//-----------------------------------------|

//Condition Stoch and S/R 1----------------|
stoch_s_r_arrow_sell = a and as and Hcon// |
//                                         |
stoch_s_r_arrow_buy = b and bs and Lcon//  |
//-----------------------------------------|
//----------------Stoch--------------------|

//-----------------RSI----------------------|
//condition RSI and dmi 1-------------------|
rsi_dmi_arrow_sell = a and ar and ad //     |
//                                          |
rsi_dmi_arrow_buy = b and br and bd //      |
//------------------------------------------|

//condition RSI and bb 1--------------------|
rsi_bb_arrow_sell = a and ar and ab //      |
//                                          |
rsi_bb_arrow_buy = b and br and bb //       |
//------------------------------------------|

//condition RSI and dmi 1-------------------|
rsi_s_r_arrow_sell = a and ar and Hcon //   |
//                                          |
rsi_s_r_arrow_buy = b and br and Lcon //    |
//------------------------------------------|
//-----------------RSI----------------------|

//----------------DMI----------------------|
//condition DMI 1--------------------------|
dmi_bb_arrow_sell = a and ad and ab //     |
//                                         |
dmi_bb_arrow_buy = b and bd and bb//       |
//-----------------------------------------|

//condition DMI 1--------------------------|
dmi_s_r_arrow_sell = a and ad and Hcon//   |
//                                         |
dmi_s_r_arrow_buy = b and bd and Lcon//    |
//-----------------------------------------|
//----------------DMI----------------------|

//----------------BB----------------------|
//condition bb  1-------------------------|
bb_s_r_arrow_sell = a and ab and Hcon //  |
//                                        |
bb_s_r_arrow_buy = b and bb and Lcon //   |
//----------------------------------------|
//----------------BB----------------------|


//ortwo*******************************************************************************************************************************************************************************************************************************************|
three_arrow_sell = stoch_rsi_arrow_sell or stoch_dmi_arrow_sell or stoch_bb_arrow_sell or stoch_s_r_arrow_sell or rsi_dmi_arrow_sell or rsi_bb_arrow_sell or rsi_s_r_arrow_sell or dmi_bb_arrow_sell or dmi_s_r_arrow_sell or bb_s_r_arrow_sell// |
//                                                                                                                                                                                                                                                | 
three_arrow_buy = stoch_rsi_arrow_buy or stoch_dmi_arrow_buy or stoch_bb_arrow_buy or stoch_s_r_arrow_buy or rsi_dmi_arrow_buy or rsi_bb_arrow_buy or rsi_s_r_arrow_buy or dmi_bb_arrow_buy or dmi_s_r_arrow_buy or bb_s_r_arrow_buy//            |
//************************************************************************************************************************************************************************************************************************************************|   

//                                      Tercera Alerta                       \\
//---------------------Stoch--------------------------|
//--------------RSI------------------------------------//
//Condition Stoch and RSI and DMI 1--------------------|
stoch_rsi_dmi_arrow_sell = a and as and ar and ad//    |
//                                                     |
stoch_rsi_dmi_arrow_buy = b and bs and br and bd//     |
//-----------------------------------------------------|

//Condition Stoch and RSI and BB 1--------------------|
stoch_rsi_bb_arrow_sell = a and as and ar and ab//    |
//                                                    |
stoch_rsi_bb_arrow_buy = b and bs and br and bb//     |
//----------------------------------------------------|

//Condition Stoch and RSI and SR 1---------------------|
stoch_rsi_s_r_arrow_sell = a and as and ar and Hcon//  |
//                                                     |
stoch_rsi_s_r_arrow_buy = b and bs and br and Lcon//   |
//-----------------------------------------------------|
//--------------RSI------------------------------------//

//--------------DMI------------------------------------//
//Condition Stoch and DMI and BB 1--------------------|
stoch_dmi_bb_arrow_sell = a and as and ad and ab//    |
//                                                    |
stoch_dmi_bb_arrow_buy = b and bs and bd and bb//     |
//----------------------------------------------------|

//Condition Stoch and DMI and SR 1---------------------|
stoch_dmi_s_r_arrow_sell = a and as and ad and Hcon//  |
//                                                     |
stoch_dmi_s_r_arrow_buy = b and bs and bd and Lcon//   |
//-----------------------------------------------------|
//--------------DMI------------------------------------//

//--------------BB-------------------------------------//
//Condition Stoch and BB and S/R 1--------------------|
stoch_bb_s_r_arrow_sell = a and as and ab and Hcon//  |
//                                                    |
stoch_bb_s_r_arrow_buy = b and bs and bb and Lcon//   |
//----------------------------------------------------|
//---------------------Stoch--------------------------|


//---------------------RSI---------------------------|
//--------------DMI------------------------------------//
//condition RSI and dmi and BB 1---------------------|
rsi_dmi_bb_arrow_sell = a and ar and ad and ab//     |
//                                                   |
rsi_dmi_bb_arrow_buy = b and br and bd and bb//      |
//---------------------------------------------------|

//condition RSI and dmi and s/r 1---------------------|
rsi_dmi_s_r_arrow_sell = a and ar and ad and Hcon//   |
//                                                    |
rsi_dmi_s_r_arrow_buy = b and br and bd and Lcon//    |
//----------------------------------------------------|
//--------------DMI------------------------------------//

//--------------BB-------------------------------------//
//condition RSI and bb and SR 1---------------------|
rsi_bb_s_r_arrow_sell = a and ar and ab and Hcon//  |
//                                                  |
rsi_bb_s_r_arrow_buy = b and br and bb and Lcon//   |
//--------------------------------------------------|
//--------------BB-------------------------------------//
//---------------------RSI---------------------------|


//---------------------DMI---------------------------|
//condition DMI and BB and s/r 1------------------------|
dmi_bb_s_r_arrow_sell = a and ad and ab and Hcon //     |
//                                                      |
dmi_bb_s_r_arrow_buy = b and bd and bb and Lcon//       |
//------------------------------------------------------|
//---------------------DMI---------------------------|

//orthree*********************************************************************************************************************************************************************************************************************************************************************************|
four_arrow_sell = stoch_rsi_dmi_arrow_sell or stoch_rsi_bb_arrow_sell or stoch_rsi_s_r_arrow_sell or stoch_dmi_bb_arrow_sell or stoch_dmi_s_r_arrow_sell or stoch_bb_s_r_arrow_sell or rsi_dmi_bb_arrow_sell or rsi_dmi_s_r_arrow_sell or rsi_bb_s_r_arrow_sell or dmi_bb_s_r_arrow_sell//|
//                                                                                                                                                                                                                                                                                        |
four_arrow_buy = stoch_rsi_dmi_arrow_buy or stoch_rsi_bb_arrow_buy or stoch_rsi_s_r_arrow_buy or stoch_dmi_bb_arrow_buy or stoch_dmi_s_r_arrow_buy or stoch_bb_s_r_arrow_buy or rsi_dmi_bb_arrow_buy or rsi_dmi_s_r_arrow_buy or rsi_bb_s_r_arrow_buy or dmi_bb_s_r_arrow_buy//           |
//****************************************************************************************************************************************************************************************************************************************************************************************|   


//                                      Cuarta Alerta                       \\
//-----------------------------------------------|
//Condition Stoch and RSI and DMI and BB1------------------------|
stoch_rsi_dmi_bb_arrow_sell = a and as and ar and ad and ab//    |
//                                                               |
stoch_rsi_dmi_bb_arrow_buy = b and bs and br and bd and bb//     |
//---------------------------------------------------------------|

//Condition Stoch and RSI and DMI and SR1--------------------------|
stoch_rsi_dmi_s_r_arrow_sell = a and as and ar and ad and Hcon//   |
//                                                                 |
stoch_rsi_dmi_s_r_arrow_buy = b and bs and br and bd and Lcon//    |
//-----------------------------------------------------------------|

//Condition Stoch and RSI and BB and SR1---------------------------|
stoch_rsi_bb_s_r_arrow_sell = a and as and ar and ab and Hcon//    |
//                                                                 |
stoch_rsi_bb_s_r_arrow_buy = b and bs and br and bb and Lcon//     |
//-----------------------------------------------------------------|

//Condition Stoch and DMI and BB and SR1---------------------------|
stoch_dmi_bb_s_r_arrow_sell = a and as and ad and ab and Hcon//    |
//                                                                 |
stoch_dmi_bb_s_r_arrow_buy = b and bs and bd and bb and Lcon//     |
//-----------------------------------------------------------------|

//Condition RSI and DMI and BB and SR1-----------------------------|
rsi_dmi_bb_s_r_arrow_sell = a and ar and ad and ab and Hcon//      |
//                                                                 |
rsi_dmi_bb_s_r_arrow_buy = b and br and bd and bb and Lcon//       |
//-----------------------------------------------------------------|
//--------------------------------------------------//

//orfour********************************************************************************************************************************************************************|
five_arrow_sell = stoch_rsi_dmi_bb_arrow_sell or stoch_rsi_dmi_s_r_arrow_sell or stoch_rsi_bb_s_r_arrow_sell or stoch_dmi_bb_s_r_arrow_sell or rsi_dmi_bb_s_r_arrow_sell//  |
//                                                                                                                                                                          |                                
five_arrow_buy = stoch_rsi_dmi_bb_arrow_buy or stoch_rsi_dmi_s_r_arrow_buy or stoch_rsi_bb_s_r_arrow_buy or stoch_dmi_bb_s_r_arrow_buy or rsi_dmi_bb_s_r_arrow_buy//        |
//**************************************************************************************************************************************************************************|


//                                      Quinta Alerta                       \\

//Condition Stoch and RSI and DMI and BB1------------------------------------|
stoch_rsi_dmi_bb_s_rarrow_sell = a and as and ar and ad and ab and Hcon//    |
//                                                                           |
stoch_rsi_dmi_bb_s_rarrow_buy = b and bs and br and bd and bb and Lcon//     |
//---------------------------------------------------------------------------|



//
//plotPrincipal
plotshape(arr ? (a and not two_arrow_sell ? a : na) : na, title="SELL", style=shape.arrowdown, location=location.abovebar, color=#f44336, transp=0, text="*PUT*\nSCORE 1", textcolor=#f44336, show_last=showlast, editable=false, size=size.small)

plotshape(arr ? (b and not two_arrow_buy ? b : na) : na, title="BUY", style=shape.arrowup, location=location.belowbar, color=#218425, transp=0, text="SCORE 1\n*CALL*", textcolor=#218425, show_last=showlast, editable=false, size=size.small)

//plot2points
plotshape(arr ? (two_arrow_sell and not three_arrow_sell ? two_arrow_sell : na) : na, title="SELL", style=shape.arrowdown, location=location.abovebar, color=#f44336, transp=0, text="*PUT*\nSCORE 2", textcolor=#f44336, show_last=showlast, editable=false, size=size.small)

plotshape(arr ? (two_arrow_buy and not three_arrow_buy ? two_arrow_buy : na) : na, title="BUY", style=shape.arrowup, location=location.belowbar, color=#218425, transp=0, text="SCORE 2\n*CALL*", textcolor=#218425, show_last=showlast, editable=false, size=size.small)


//Plot3Points
plotshape(arr ? (three_arrow_sell and not four_arrow_sell ? three_arrow_sell : na) : na, title="SELL", style=shape.arrowdown, location=location.abovebar, color=#f44336, transp=0, text="*PUT*\nSCORE 3", textcolor=#f44336, show_last=showlast, editable=false, size=size.small)

plotshape(arr ? (three_arrow_buy and not four_arrow_buy ? three_arrow_buy : na) : na, title="BUY", style=shape.arrowup, location=location.belowbar, color=#218425, transp=0, text="SCORE 3\n*CALL*", textcolor=#218425, show_last=showlast, editable=false, size=size.small)


//Plot4Points
plotshape(arr ? (four_arrow_sell and not five_arrow_sell ? four_arrow_sell : na) : na, title="SELL", style=shape.arrowdown, location=location.abovebar, color=#f44336, transp=0, text="*PUT*\nSCORE 4", textcolor=#f44336, show_last=showlast, editable=false, size=size.small)

plotshape(arr ? (four_arrow_buy and not five_arrow_buy ? four_arrow_buy : na) : na, title="BUY", style=shape.arrowup, location=location.belowbar, color=#218425, transp=0, text="SCORE 4\n*CALL*", textcolor=#218425, show_last=showlast, editable=false, size=size.small)


//plot5points
plotshape(arr ? (five_arrow_sell and not stoch_rsi_dmi_bb_s_rarrow_sell ? five_arrow_sell : na) : na , title="SELL", style=shape.arrowdown, location=location.abovebar, color=#f44336, transp=0, text="*PUT*\nSCORE 5", textcolor=#f44336, show_last=showlast, editable=false, size=size.small)

plotshape(arr ? (five_arrow_buy and not stoch_rsi_dmi_bb_s_rarrow_buy ? five_arrow_buy : na) : na, title="BUY", style=shape.arrowup, location=location.belowbar, color=#218425, transp=0, text="SCORE 5\n*CALL*", textcolor=#218425, show_last=showlast, editable=false, size=size.small)

//plot6points
plotshape(arr ? stoch_rsi_dmi_bb_s_rarrow_sell : na, title="SELL", style=shape.arrowdown, location=location.abovebar, color=#f44336, transp=0, text="*PUT*\nSCORE 6", textcolor=#f44336, show_last=showlast, editable=false, size=size.small)

plotshape(arr ? stoch_rsi_dmi_bb_s_rarrow_buy : na, title="BUY", style=shape.arrowup, location=location.belowbar, color=#218425, transp=0, text="SCORE 6\n*CALL*", textcolor=#218425, show_last=showlast, editable=false, size=size.small)


start = 0.02
increment = 0.02
maximum = 0.2
out50 = sar(start, increment, maximum)
plot(out50, "ParabolicSAR", style=plot.style_cross, color=#000000, linewidth=1, display = display.none)