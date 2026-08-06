# coupon-analysis
## Overview - Will a customer accept the coupon?

The goal is to understand what drives a driver to accept or reject a coupon, using bar coupons and coffee house coupons as the primary case studies, and to translate those patterns into interpretable hypotheses.

## Dataset
Target variable: Y — binary, 1 if the driver accepted the coupon, 0 if rejected.
Coupon types: coupon column — Bar, Coffee House, Restaurant(<20), Restaurant(20-50), Carry out & Take away.
Contextual features: time, weather, temperature, passanger, destination, direction_same, toCoupon_GEQ5min/15min/25min.
Behavioral features: Bar, CoffeeHouse, RestaurantLessThan20, Restaurant20To50 — self-reported frequency of visiting each venue type.
Demographic features: age, maritalStatus, income, occupation, education, gender, has_children.
