
### 文字列「yyyymmdd_hhmmss」を得る

``` sas
%local _timestamp;
data _null_;
    call symputx("_timestamp", put(today(), yymmddn8.) || '_' || compress(put(datetime(), tod8.), ':'));
run;
```
