```bash

SELECT @SelectedDate = CAST(CONCAT(@Year, '-', @Month, '-01') AS DATE);
	
SELECT 
    @FirstDate = CAST(@SelectedDate AS DATE),
    @LastDate = EOMONTH(@SelectedDate);

WITH DateList AS (
    SELECT CAST(@FirstDate AS DATE) AS DateValue
    UNION ALL
    SELECT DATEADD(DAY, 1, DateValue)
    FROM DateList
    WHERE DateValue < @LastDate
)
SELECT DateValue
FROM DateList
OPTION (MAXRECURSION 0);  -- Allows recursion to continue beyond the default limit
