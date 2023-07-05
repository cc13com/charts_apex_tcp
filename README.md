# Charts APEX Template Component Plugin

The Charts APEX Template Component Plugin allows you to display charts in each column of your APEX reports. This plugin uses Charts.js library from GitHub project https://github.com/chartjs/Chart.js

This Template Component Plugin supports "Single (Partial)" usage only. It can be referenced in a report column.

# Example
![Chart_Plugin_Example](https://github.com/cc13com/charts_apex_tcp/assets/35263232/cdf489d1-363e-4173-824f-46d9438c2c98)

# Usage

Create a Classic or Interactive Report in your APEX application. The Source (Table or SQL Query) has to deliver all information needed to fill the chart with information (x- and y-axis). See the example where the column alias "measured_values" fetch the values for the y-axis and the column alias "date_values" fetch the values for the x-axis.

```
  select a.ID,
       a.SYMBOL,
       a.long_name,
       a.AMOUNT,
       'chart' as chart,
       'Date' as xlabel,
       'Value' as ylabel,
        (   select listagg(regularmarketpreviousclose, ';') within group(order by datum) as closevalues
                from st_stocks_historie
                where trunc(datum) > TRUNC (SYSDATE, 'YEAR')
                --where trunc(datum) > TRUNC (SYSDATE)-30
                and stock_id = a.id
                 group by symbol
            ) as measured_values,
            (   select listagg(to_char(datum, 'DD.MM.YYYY'), ';') within group(order by datum) as closedate
                from st_stocks_historie
                where trunc(datum) > TRUNC (SYSDATE, 'YEAR')
                --where trunc(datum) > TRUNC (SYSDATE)-30
                and stock_id = a.id
                 group by symbol
            ) as date_values
  from ST_STOCKS a, st_stocks_year_performance b, v_stocks_week_performance c
  order by a.long_name
```

With the two column aliases "xlabel" and "ylabel" in this example we set the values for the chart labels. You will find all of this aliases as parameters in the plugin.



# Installation

* Download the Plugin file 'template_component_plugin_com_cc13_charts.sql" from the latest release
* Import the Plugin file into your application
