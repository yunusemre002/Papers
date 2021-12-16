# Add Brackets for Cells by Formula in [Excel](https://www.excelhow.net/how-to-add-brackets-for-cells-in-excel.html)
`=”(“&A2&”)”`
```
A          B
YEAR      YEAR
2021    =”(“&A2&”)”   ->   (2021)
2022    =”(“&A3&”)”   ->   (2022)
```

# Normalize [text](https://exceljet.net/formula/normalize-text)
`=LOWER(TRIM(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A1,"(",""),")",""),"-",""),":",""),";",""),"_",""),",",""),".","")))`

```
DEPARTMENT_id_anD-Na.me     ->  departmentidandname
```

```
=
LOWER(
TRIM(
SUBSTITUTE(
SUBSTITUTE(
SUBSTITUTE(
SUBSTITUTE(
SUBSTITUTE(
SUBSTITUTE(
SUBSTITUTE(
SUBSTITUTE(
A1,
"("," "),
")"," "),
"-"," "),
":"," "),
";"," "),
"_"," "),
","," "),
"."," ")))
```


