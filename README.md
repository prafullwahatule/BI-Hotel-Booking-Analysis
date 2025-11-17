# 🏨 Hotel Booking Analysis -- Power BI Project

## 📌 Project Overview

This project is a **Power BI-based interactive analytics solution**
designed to uncover insights from hotel booking data.\
The dashboard provides deep visibility into **guest behavior 👥, booking
trends 📅, room & meal performance 🛏️🍽️, distribution channels 🌐,
cancellations ❌, and revenue insights 💰**.\
It enables hotel managers to make **data‑driven decisions**, optimize
operations, and reduce revenue loss.

The project includes **data cleaning, transformation, modeling, DAX
measures, KPIs**, and **four fully interactive dashboard pages**.

------------------------------------------------------------------------

## ❗ Problem Statement

Hotels receive thousands of bookings through multiple channels but
struggle with:

-   High cancellation and no-show rates affecting revenue\
-   Lack of visibility into guest demographics and booking patterns\
-   Difficulty identifying top-performing room types, meal plans &
    channels\
-   No clear measurement of revenue lost due to cancellations\
-   Limited insight into repeat guest behavior & loyalty\
-   Hard to forecast demand due to scattered lead time and seasonal
    trends\
-   Uncertainty about which booking channels are profitable or risky

Due to these issues, hotels face challenges in making **strategic
decisions**, **optimizing operations**, and **increasing
profitability**.

------------------------------------------------------------------------

## 🎯 Project Objectives

-   **Centralize Booking Data 🗂️:** Create a unified, analytics-ready
    dataset.\
-   **Understand Guest Behavior 👥:** Analyze customer types, repeat
    guests, and preferences.\
-   **Track Key Hotel Metrics 📊:** Total bookings, guests, revenue,
    ADR, room utilization.\
-   **Analyze Room & Meal Performance 🛏️🍽️:** Popular room types, meal
    plans, upgrade patterns.\
-   **Evaluate Channel Performance 🌐:** Revenue, cancellations, volume
    across channels.\
-   **Understand Cancellations ❌:** Patterns by month, customer type,
    deposit type, channel.\
-   **Measure Revenue Loss 💰:** Calculate revenue leakage & retention.\
-   **Support Decision‑Making 🧠:** Provide interactive dashboards for
    insights.

------------------------------------------------------------------------

# 🧹 Data Cleaning Summary

Total **119,390 raw rows** cleaned → Final dataset **84,755 rows**, **28
columns**

### ✔ Key Cleaning Activities

  ---------------------------------------------------------------------------
  Step          Description           Before        After       Effect
  ------------- --------------------- ------------- ----------- -------------
  Remove Errors Fixed invalid entries 1,19,390      1,19,386    -4 rows

  Remove Blank  Removed empty rows    1,19,386      1,19,386    No change
  Rows                                                          

  Remove        Removed duplicate     1,19,386      87,392      -31,994 rows
  Duplicates    records                                         

  Add Primary   Introduced unique     32 cols       33 cols     +1 column
  Key           booking ID                                      

  Create        adults + children +   87,392        87,220      -172 rows
  total_guest   babies                                          invalid

  Remove        company, car parking, 34 cols       28 cols     -6 columns
  Columns       special requests                                

  Final Cleanup recheck error/blank   87,220        86,605      -615 rows
                duplicates                                      

  Fix guest     Corrected             86,605        84,755      -1850 rows
  errors        guest-related records                           
  ---------------------------------------------------------------------------

### 📌 Final Dataset Shape

-   **Rows:** 84,755\
-   **Columns:** 28

------------------------------------------------------------------------

# 🔄 Transformation & Data Model

### ✔ Key Transformations

-   Added **Primary Key** (booking_id)\
-   Merged arrival year, month, day → **arrival_date**\
-   Removed: week number, company, parking, special request\
-   Created **total_guest = adults + children + babies**\
-   Replaced NULLs:
    -   agent → "Direct Booking"\
    -   meal → "UD"\
    -   distribution_channel → "UD"\
-   Added **Total Nights = stays_in_weekend_nights +
    stays_in_week_nights**

------------------------------------------------------------------------

# 🧮 DAX Measures

### **KPI Measures**

    Total Booking = DISTINCTCOUNT('Hotel Bookings'[booking_id])

    Total Guests = SUM('Hotel Bookings'[total_guest])

    Total Revenue =
    SUMX(
        'Hotel Bookings',
        'Hotel Bookings'[adr] * 'Hotel Bookings'[Total Nights]
    )

    Average ADR = AVERAGE('Hotel Bookings'[adr])

    Repeat Guest % =
    DIVIDE(
        CALCULATE(COUNTROWS('Hotel Bookings'),'Hotel Bookings'[is_repeated_guest] = 1),
        COUNTROWS('Hotel Bookings')
    )

    Avg Lead Time per Customer Type =
    AVERAGEX(
        VALUES('Hotel Bookings'[customer_type]),
        CALCULATE(AVERAGE('Hotel Bookings'[lead_time]))
    )

    Average Guests per Booking = AVERAGE('Hotel Bookings'[total_guest])

### **Cancellation & Revenue Measures**

    No Show % =
    DIVIDE(
        CALCULATE(COUNTROWS('Hotel Bookings'),'Hotel Bookings'[reservation_status] = "No-Show"),
        COUNTROWS('Hotel Bookings')
    ) * 100

    Revenue Lost Due to Cancellation =
    SUMX(
        FILTER('Hotel Bookings','Hotel Bookings'[is_canceled] = 1),
        'Hotel Bookings'[adr] * 'Hotel Bookings'[Total Nights]
    )

    Revenue Retention % =
    DIVIDE(
        [Total Revenue] - [Revenue Lost Due to Cancellation],
        [Total Revenue]
    ) * 100

------------------------------------------------------------------------

# 📊 Dashboard Pages Details

## 📌 Page 1 --- Overview & Key Insights

-   KPIs: Total Bookings, Guests, Revenue, ADR\
-   Visuals: Monthly Trend, ADR vs Lead Time, Hotel Type Share,
    Reservation Status
<img width="1366" height="769" alt="Screenshot 1" src="https://github.com/user-attachments/assets/a879b585-3f8a-4d27-9c11-9e0a4d12f0c4" />

## 📌 Page 2 --- Guest & Booking Analysis

-   KPIs: Total Guests, Repeat %, Avg Lead Time, Avg Guests\
-   Visuals: Guest Type, Repeat vs New, Lead Time, Customer Type Cancel
    %, Country Map
<img width="1368" height="772" alt="Screenshot 2" src="https://github.com/user-attachments/assets/2e860426-21a4-44f0-b754-77f4e5ba4af7" />

## 📌 Page 3 --- Room, Meal & Channel Performance

-   KPIs: Meal Popularity %, Channel Revenue %, Room Revenue\
-   Visuals: Meal Plan, Cancellation % by Channel, Market Segment ADR,
    Room Revenue, Channel Trends
<img width="1366" height="771" alt="Screenshot 3" src="https://github.com/user-attachments/assets/6fed37f6-2275-4b90-8276-fca2f928ebdb" />

## 📌 Page 4 --- Cancellation & Revenue Insights

-   KPIs: No-Show %, Revenue Lost, Revenue Retention\
-   Visuals: Monthly Cancellation Trend, Deposit Type vs Cancellation,
    Monthly Revenue Lost, Channel Cancel %
<img width="1367" height="771" alt="Screenshot 4" src="https://github.com/user-attachments/assets/de3ce0d9-2584-4bb8-a9ed-4e3c339cc2c1" />

------------------------------------------------------------------------

# 🛠️ How to Use

-   Open `.pbix` file in Power BI\
-   Navigate 4 report pages\
-   Use slicers (Hotel Type, Customer Type, Year, Market Segment...)\
-   Hover for tooltips\
-   Export → PDF / Image

------------------------------------------------------------------------

# 🏁 Conclusion

This project converts raw hotel booking data into **actionable
insights** through a professional Power BI dashboard.

Hotels can now:

✔ Reduce cancellations\
✔ Improve guest experience\
✔ Identify profitable channels\
✔ Enhance room & meal planning\
✔ Boost revenue with strategic insights

------------------------------------------------------------------------

# 🛠️ Tools Used

-   **Power BI Desktop**\
-   **Power Query Editor**\
-   **DAX**\
-   **Excel / CSV Dataset**

------------------------------------------------------------------------

# 🙏 Acknowledgement

Thanks to the **public Hotel Booking Dataset** for enabling real‑world
analytics.

------------------------------------------------------------------------

## 📎 Author  

**👤 Name:** Prafull Wahatule  
**📧 Email:** [prafullwahatule@gmail.com](mailto:prafullwahatule@gmail.com)  
**💻 GitHub:** [prafullwahatule](https://github.com/prafullwahatule)  

------------------------------------------------------------------------

⭐ If you found this helpful, please star the repository! ⭐
