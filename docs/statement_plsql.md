# Oracle SQL statements

Mostly copied from w3schools.com/sql/

## Get First and Last day of curent month
To get first and last days of the month of a given date:

	select
		trunc(sysdate) - (to_number(to_char(sysdate,'DD')) - 1) "First Day",
		add_months(trunc(sysdate) - (to_number(to_char(sysdate,'DD')) - 1), 1) -1 "Last Day"
	from dual;