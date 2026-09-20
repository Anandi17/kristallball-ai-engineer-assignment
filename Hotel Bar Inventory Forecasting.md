### **Hotel Bar Inventory Forecasting and Par Level Recommendation System**



**1. Core business problem and why it matters**



Hotel bars face a trade-off between availability and inventory cost. If a popular brand is unavailable, the hotel may lose a sale, disappoint a guest, or force the bartender to offer a substitute. If the bar holds too much of a slow-moving product, cash is tied up in stock, storage space is consumed, and the risk of shrinkage, breakage, and expiry increases.



This solution helps managers make a consistent daily decision for every bar-brand combination: how much inventory should be available to cover expected demand and supplier lead time. The dataset covers six bars and sixteen brands. The system converts transaction-level inventory movements into daily demand forecasts, then turns those forecasts into recommended par levels.



**2. Assumptions made and why**



The model assumes a fixed two-day supplier lead time. This is a practical starting point because supplier lead-time data was not included in the dataset. In production, replace this with supplier-specific lead-time distributions.



The inventory policy targets a 95% cycle-service level, using a Z-score of 1.645. This provides a reasonable balance between avoiding stockouts and avoiding excessive safety stock. A premium or high-volume outlet could use a higher target, while slow-moving products could use a lower target.



The system reviews inventory daily and orders in 30 ml increments. It assumes unmet demand is lost rather than backordered because a guest who cannot order a drink is unlikely to return later to complete the purchase.



All inventory measures are treated as millilitres. The system also assumes the reported consumed quantity represents demand. This is an important limitation: when a bar is out of stock, recorded consumption can understate true guest demand. For this reason, zero closing balances are treated as stockout proxies rather than proof of lost sales.



**3. Model selection and rationale**



The system compares two transparent, causal forecasting models for each bar-brand series:



1\. A seven-day moving average.

2\. A rolling same-weekday seasonal average.



The seven-day moving average provides a stable baseline by smoothing recent demand. The same-weekday model captures weekly patterns, such as demand differences between weekends and weekdays. For each bar-brand combination, the system selects the model with the lower WAPE during the training period.



I chose these methods because they are easy to explain, work with zero-demand days, and can be recalculated quickly for many bar-brand combinations. The final 20% of dates are held out chronologically for testing, so the future is never used to predict the past.



We did not use more complex approaches, such as Holt-Winters, ARIMA, or machine-learning models with lag features, as the primary model because the data is relatively short, demand is intermittent, and no external drivers are available. Those models may improve accuracy when the hotel adds promotion calendars, holidays, occupancy, events, weather, or richer point-of-sale data.



**4. Performance and improvements**



All 6,575 source records passed the inventory conservation check:



`Closing Balance = Opening Balance + Purchase - Consumed`



The final holdout WAPE was 169.3%, which indicates that demand is highly volatile and intermittent at the bar-brand level. This result reinforces the need for safety stock and exception management rather than blind reliance on a point forecast.



Using a two-day lead time and a 95% service target, the historical holdout simulation recorded 255 stockout days, 42,874 ml of unmet observed demand, and average closing inventory of 467 ml. These figures should be treated as a policy scenario, not as a claimed improvement over historical performance, because observed consumption may already be constrained by historical stockouts.



The next improvements should include:



\- Capturing missed sales and substitutions when an item is unavailable.

\- Using actual supplier lead times and late-delivery history.

\- Adding promotion, holiday, event, weather, and hotel-occupancy data.

\- Testing intermittent-demand methods and lag-feature machine-learning models.

\- Applying different service targets by item velocity, margin, and guest importance.



**5. How the solution would work in a real hotel**



Each morning, the hotel’s inventory and POS data would refresh the daily demand series. The forecasting process would calculate the next demand estimate, forecast error, safety stock, and par level for every bar-brand combination.



Managers would see a daily recommendation containing the current inventory position, expected demand, recommended par level, and order quantity. Inventory position would include both on-hand stock and confirmed incoming supplier orders. If inventory position is below par, the system recommends ordering the difference.



The system should not fully automate purchasing without controls. It should alert managers when there are repeated stockout proxies, unusually large forecast errors, missing transactions, planned promotions, or supplier delays. Over time, managers can adjust service-level targets by bar and item category, while the data team monitors WAPE, stockout rate, inventory days, shrinkage, and ordering accuracy.

