# Deliverable 4 SQL: Full Table Names and Alias Reference

Use the **Full table names** version for the Deliverable 4 screenshots and written explanations. The **Table aliases** version is included as a future reference for shortening join queries.

A table alias is introduced in the FROM or JOIN clause. For example, CUSTOMER AS C permits later references to CUSTOMER.CUSTOMER_ID to be shortened to C.CUSTOMER_ID. Both versions produce the same result.

Names such as PURCHASE_COUNT and PAYMENT_PATH are descriptive **column aliases** for query results. They have been retained because they make the result headings easier to understand; they are not abbreviated table names.

## Query 1 — Customer purchase totals

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    CUSTOMER.CUSTOMER_ID,<br>    CUSTOMER.CUST_FIRST_NAME,<br>    CUSTOMER.CUST_LAST_NAME,<br>    COUNT(SALE.SALE_ID) AS PURCHASE_COUNT<br>FROM CUSTOMER<br>LEFT JOIN SALE<br>    ON SALE.CUSTOMER_ID = CUSTOMER.CUSTOMER_ID<br>GROUP BY<br>    CUSTOMER.CUSTOMER_ID,<br>    CUSTOMER.CUST_FIRST_NAME,<br>    CUSTOMER.CUST_LAST_NAME<br>ORDER BY<br>    PURCHASE_COUNT DESC,<br>    CUSTOMER.CUSTOMER_ID;</code></pre> | <pre><code>SELECT<br>    C.CUSTOMER_ID,<br>    C.CUST_FIRST_NAME,<br>    C.CUST_LAST_NAME,<br>    COUNT(S.SALE_ID) AS PURCHASE_COUNT<br>FROM CUSTOMER AS C<br>LEFT JOIN SALE AS S<br>    ON S.CUSTOMER_ID = C.CUSTOMER_ID<br>GROUP BY<br>    C.CUSTOMER_ID,<br>    C.CUST_FIRST_NAME,<br>    C.CUST_LAST_NAME<br>ORDER BY<br>    PURCHASE_COUNT DESC,<br>    C.CUSTOMER_ID;</code></pre> |

Alias mapping: C = CUSTOMER; S = SALE.

## Query 2 — Vehicle sales history

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    VEHICLE.VIN,<br>    VEHICLE.VEH_YEAR,<br>    VEHICLE.VEH_MAKE,<br>    VEHICLE.VEH_MODEL,<br>    VEHICLE.VEH_STATUS,<br>    COUNT(SALE.SALE_ID) AS TIMES_SOLD<br>FROM VEHICLE<br>LEFT JOIN SALE<br>    ON SALE.VIN = VEHICLE.VIN<br>GROUP BY<br>    VEHICLE.VIN,<br>    VEHICLE.VEH_YEAR,<br>    VEHICLE.VEH_MAKE,<br>    VEHICLE.VEH_MODEL,<br>    VEHICLE.VEH_STATUS<br>ORDER BY<br>    TIMES_SOLD DESC,<br>    VEHICLE.VIN;</code></pre> | <pre><code>SELECT<br>    V.VIN,<br>    V.VEH_YEAR,<br>    V.VEH_MAKE,<br>    V.VEH_MODEL,<br>    V.VEH_STATUS,<br>    COUNT(S.SALE_ID) AS TIMES_SOLD<br>FROM VEHICLE AS V<br>LEFT JOIN SALE AS S<br>    ON S.VIN = V.VIN<br>GROUP BY<br>    V.VIN,<br>    V.VEH_YEAR,<br>    V.VEH_MAKE,<br>    V.VEH_MODEL,<br>    V.VEH_STATUS<br>ORDER BY<br>    TIMES_SOLD DESC,<br>    V.VIN;</code></pre> |

Alias mapping: V = VEHICLE; S = SALE.

## Query 3 — Financing path for every sale

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    SALE.SALE_ID,<br>    SALE.SALE_DATE,<br>    CUSTOMER.CUST_FIRST_NAME,<br>    CUSTOMER.CUST_LAST_NAME,<br>    VEHICLE.VEH_MAKE,<br>    VEHICLE.VEH_MODEL,<br>    FINANCING_ACCOUNT.ACCOUNT_ID,<br>    CASE<br>        WHEN FINANCING_ACCOUNT.ACCOUNT_ID IS NULL<br>            THEN 'Cash'<br>        WHEN FINANCING_ACCOUNT.ACCT_TYPE = 'I'<br>             AND INHOUSE_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>             AND EXTERNAL_ACCOUNT.ACCOUNT_ID IS NULL<br>            THEN 'In-House'<br>        WHEN FINANCING_ACCOUNT.ACCT_TYPE = 'E'<br>             AND EXTERNAL_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>             AND INHOUSE_ACCOUNT.ACCOUNT_ID IS NULL<br>            THEN 'External Lender'<br>        ELSE 'Rule Violation'<br>    END AS PAYMENT_PATH<br>FROM SALE<br>JOIN CUSTOMER<br>    ON CUSTOMER.CUSTOMER_ID = SALE.CUSTOMER_ID<br>JOIN VEHICLE<br>    ON VEHICLE.VIN = SALE.VIN<br>LEFT JOIN FINANCING_ACCOUNT<br>    ON FINANCING_ACCOUNT.SALE_ID = SALE.SALE_ID<br>LEFT JOIN INHOUSE_ACCOUNT<br>    ON INHOUSE_ACCOUNT.ACCOUNT_ID =<br>       FINANCING_ACCOUNT.ACCOUNT_ID<br>LEFT JOIN EXTERNAL_ACCOUNT<br>    ON EXTERNAL_ACCOUNT.ACCOUNT_ID =<br>       FINANCING_ACCOUNT.ACCOUNT_ID<br>ORDER BY SALE.SALE_ID;</code></pre> | <pre><code>SELECT<br>    S.SALE_ID,<br>    S.SALE_DATE,<br>    C.CUST_FIRST_NAME,<br>    C.CUST_LAST_NAME,<br>    V.VEH_MAKE,<br>    V.VEH_MODEL,<br>    FA.ACCOUNT_ID,<br>    CASE<br>        WHEN FA.ACCOUNT_ID IS NULL THEN 'Cash'<br>        WHEN FA.ACCT_TYPE = 'I'<br>             AND IA.ACCOUNT_ID IS NOT NULL<br>             AND EA.ACCOUNT_ID IS NULL<br>            THEN 'In-House'<br>        WHEN FA.ACCT_TYPE = 'E'<br>             AND EA.ACCOUNT_ID IS NOT NULL<br>             AND IA.ACCOUNT_ID IS NULL<br>            THEN 'External Lender'<br>        ELSE 'Rule Violation'<br>    END AS PAYMENT_PATH<br>FROM SALE AS S<br>JOIN CUSTOMER AS C<br>    ON C.CUSTOMER_ID = S.CUSTOMER_ID<br>JOIN VEHICLE AS V<br>    ON V.VIN = S.VIN<br>LEFT JOIN FINANCING_ACCOUNT AS FA<br>    ON FA.SALE_ID = S.SALE_ID<br>LEFT JOIN INHOUSE_ACCOUNT AS IA<br>    ON IA.ACCOUNT_ID = FA.ACCOUNT_ID<br>LEFT JOIN EXTERNAL_ACCOUNT AS EA<br>    ON EA.ACCOUNT_ID = FA.ACCOUNT_ID<br>ORDER BY S.SALE_ID;</code></pre> |

Alias mapping: S = SALE; C = CUSTOMER; V = VEHICLE; FA = FINANCING_ACCOUNT; IA = INHOUSE_ACCOUNT; EA = EXTERNAL_ACCOUNT.

## Query 4 — Financing subtype validation

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    FINANCING_ACCOUNT.ACCOUNT_ID,<br>    FINANCING_ACCOUNT.ACCT_TYPE,<br>    CASE<br>        WHEN INHOUSE_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>            THEN 'In-House'<br>        WHEN EXTERNAL_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>            THEN 'External'<br>        ELSE 'Missing Subtype'<br>    END AS SUBTYPE_FOUND,<br>    CASE<br>        WHEN FINANCING_ACCOUNT.ACCT_TYPE = 'I'<br>             AND INHOUSE_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>             AND EXTERNAL_ACCOUNT.ACCOUNT_ID IS NULL<br>            THEN 'Valid'<br>        WHEN FINANCING_ACCOUNT.ACCT_TYPE = 'E'<br>             AND EXTERNAL_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>             AND INHOUSE_ACCOUNT.ACCOUNT_ID IS NULL<br>            THEN 'Valid'<br>        ELSE 'Violation'<br>    END AS RULE_4_CHECK<br>FROM FINANCING_ACCOUNT<br>LEFT JOIN INHOUSE_ACCOUNT<br>    ON INHOUSE_ACCOUNT.ACCOUNT_ID =<br>       FINANCING_ACCOUNT.ACCOUNT_ID<br>LEFT JOIN EXTERNAL_ACCOUNT<br>    ON EXTERNAL_ACCOUNT.ACCOUNT_ID =<br>       FINANCING_ACCOUNT.ACCOUNT_ID<br>ORDER BY FINANCING_ACCOUNT.ACCOUNT_ID;</code></pre> | <pre><code>SELECT<br>    FA.ACCOUNT_ID,<br>    FA.ACCT_TYPE,<br>    CASE<br>        WHEN IA.ACCOUNT_ID IS NOT NULL<br>            THEN 'In-House'<br>        WHEN EA.ACCOUNT_ID IS NOT NULL<br>            THEN 'External'<br>        ELSE 'Missing Subtype'<br>    END AS SUBTYPE_FOUND,<br>    CASE<br>        WHEN FA.ACCT_TYPE = 'I'<br>             AND IA.ACCOUNT_ID IS NOT NULL<br>             AND EA.ACCOUNT_ID IS NULL<br>            THEN 'Valid'<br>        WHEN FA.ACCT_TYPE = 'E'<br>             AND EA.ACCOUNT_ID IS NOT NULL<br>             AND IA.ACCOUNT_ID IS NULL<br>            THEN 'Valid'<br>        ELSE 'Violation'<br>    END AS RULE_4_CHECK<br>FROM FINANCING_ACCOUNT AS FA<br>LEFT JOIN INHOUSE_ACCOUNT AS IA<br>    ON IA.ACCOUNT_ID = FA.ACCOUNT_ID<br>LEFT JOIN EXTERNAL_ACCOUNT AS EA<br>    ON EA.ACCOUNT_ID = FA.ACCOUNT_ID<br>ORDER BY FA.ACCOUNT_ID;</code></pre> |

Alias mapping: FA = FINANCING_ACCOUNT; IA = INHOUSE_ACCOUNT; EA = EXTERNAL_ACCOUNT.

## Query 5 — Accounts funded by each external lender

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    EXTERNAL_LENDER.LENDER_ID,<br>    EXTERNAL_LENDER.LENDER_NAME,<br>    COUNT(EXTERNAL_ACCOUNT.ACCOUNT_ID)<br>        AS FUNDED_ACCOUNTS,<br>    COALESCE(<br>        SUM(EXTERNAL_ACCOUNT.EXT_CONTRACT_PURCHASE_AMT),<br>        0.00<br>    ) AS TOTAL_CONTRACTS_PURCHASED<br>FROM EXTERNAL_LENDER<br>LEFT JOIN EXTERNAL_ACCOUNT<br>    ON EXTERNAL_ACCOUNT.LENDER_ID =<br>       EXTERNAL_LENDER.LENDER_ID<br>GROUP BY<br>    EXTERNAL_LENDER.LENDER_ID,<br>    EXTERNAL_LENDER.LENDER_NAME<br>ORDER BY<br>    FUNDED_ACCOUNTS DESC,<br>    EXTERNAL_LENDER.LENDER_ID;</code></pre> | <pre><code>SELECT<br>    L.LENDER_ID,<br>    L.LENDER_NAME,<br>    COUNT(EA.ACCOUNT_ID) AS FUNDED_ACCOUNTS,<br>    COALESCE(<br>        SUM(EA.EXT_CONTRACT_PURCHASE_AMT),<br>        0.00<br>    ) AS TOTAL_CONTRACTS_PURCHASED<br>FROM EXTERNAL_LENDER AS L<br>LEFT JOIN EXTERNAL_ACCOUNT AS EA<br>    ON EA.LENDER_ID = L.LENDER_ID<br>GROUP BY<br>    L.LENDER_ID,<br>    L.LENDER_NAME<br>ORDER BY<br>    FUNDED_ACCOUNTS DESC,<br>    L.LENDER_ID;</code></pre> |

Alias mapping: L = EXTERNAL_LENDER; EA = EXTERNAL_ACCOUNT.

## Query 6 — Payment summary for in-house accounts

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    INHOUSE_ACCOUNT.ACCOUNT_ID,<br>    CUSTOMER.CUST_FIRST_NAME,<br>    CUSTOMER.CUST_LAST_NAME,<br>    COUNT(PAYMENT.PAYMENT_ID) AS PAYMENT_COUNT,<br>    COALESCE(SUM(PAYMENT.PMT_AMOUNT), 0.00)<br>        AS TOTAL_RECEIVED,<br>    INHOUSE_ACCOUNT.INH_NEXT_DUE_DATE,<br>    INHOUSE_ACCOUNT.INH_DELINQUENCY_STATUS<br>FROM INHOUSE_ACCOUNT<br>JOIN FINANCING_ACCOUNT<br>    ON FINANCING_ACCOUNT.ACCOUNT_ID =<br>       INHOUSE_ACCOUNT.ACCOUNT_ID<br>JOIN SALE<br>    ON SALE.SALE_ID = FINANCING_ACCOUNT.SALE_ID<br>JOIN CUSTOMER<br>    ON CUSTOMER.CUSTOMER_ID = SALE.CUSTOMER_ID<br>LEFT JOIN PAYMENT<br>    ON PAYMENT.ACCOUNT_ID = INHOUSE_ACCOUNT.ACCOUNT_ID<br>GROUP BY<br>    INHOUSE_ACCOUNT.ACCOUNT_ID,<br>    CUSTOMER.CUST_FIRST_NAME,<br>    CUSTOMER.CUST_LAST_NAME,<br>    INHOUSE_ACCOUNT.INH_NEXT_DUE_DATE,<br>    INHOUSE_ACCOUNT.INH_DELINQUENCY_STATUS<br>ORDER BY INHOUSE_ACCOUNT.ACCOUNT_ID;</code></pre> | <pre><code>SELECT<br>    IA.ACCOUNT_ID,<br>    C.CUST_FIRST_NAME,<br>    C.CUST_LAST_NAME,<br>    COUNT(P.PAYMENT_ID) AS PAYMENT_COUNT,<br>    COALESCE(SUM(P.PMT_AMOUNT), 0.00)<br>        AS TOTAL_RECEIVED,<br>    IA.INH_NEXT_DUE_DATE,<br>    IA.INH_DELINQUENCY_STATUS<br>FROM INHOUSE_ACCOUNT AS IA<br>JOIN FINANCING_ACCOUNT AS FA<br>    ON FA.ACCOUNT_ID = IA.ACCOUNT_ID<br>JOIN SALE AS S<br>    ON S.SALE_ID = FA.SALE_ID<br>JOIN CUSTOMER AS C<br>    ON C.CUSTOMER_ID = S.CUSTOMER_ID<br>LEFT JOIN PAYMENT AS P<br>    ON P.ACCOUNT_ID = IA.ACCOUNT_ID<br>GROUP BY<br>    IA.ACCOUNT_ID,<br>    C.CUST_FIRST_NAME,<br>    C.CUST_LAST_NAME,<br>    IA.INH_NEXT_DUE_DATE,<br>    IA.INH_DELINQUENCY_STATUS<br>ORDER BY IA.ACCOUNT_ID;</code></pre> |

Alias mapping: IA = INHOUSE_ACCOUNT; FA = FINANCING_ACCOUNT; S = SALE; C = CUSTOMER; P = PAYMENT.

## Query 7 — Verify that payments are in-house only

| Full table names — use for assignment | Table aliases — reference version |
|---|---|
| <pre><code>SELECT<br>    PAYMENT.PAYMENT_ID,<br>    PAYMENT.PMT_RECEIPT_NUMBER,<br>    PAYMENT.ACCOUNT_ID,<br>    FINANCING_ACCOUNT.ACCT_TYPE,<br>    PAYMENT.PMT_AMOUNT,<br>    CASE<br>        WHEN INHOUSE_ACCOUNT.ACCOUNT_ID IS NOT NULL<br>             AND FINANCING_ACCOUNT.ACCT_TYPE = 'I'<br>             AND EXTERNAL_ACCOUNT.ACCOUNT_ID IS NULL<br>            THEN 'Valid - In-House Only'<br>        ELSE 'Violation'<br>    END AS RULE_7_CHECK<br>FROM PAYMENT<br>LEFT JOIN INHOUSE_ACCOUNT<br>    ON INHOUSE_ACCOUNT.ACCOUNT_ID = PAYMENT.ACCOUNT_ID<br>LEFT JOIN FINANCING_ACCOUNT<br>    ON FINANCING_ACCOUNT.ACCOUNT_ID =<br>       INHOUSE_ACCOUNT.ACCOUNT_ID<br>LEFT JOIN EXTERNAL_ACCOUNT<br>    ON EXTERNAL_ACCOUNT.ACCOUNT_ID = PAYMENT.ACCOUNT_ID<br>ORDER BY PAYMENT.PAYMENT_ID;</code></pre> | <pre><code>SELECT<br>    P.PAYMENT_ID,<br>    P.PMT_RECEIPT_NUMBER,<br>    P.ACCOUNT_ID,<br>    FA.ACCT_TYPE,<br>    P.PMT_AMOUNT,<br>    CASE<br>        WHEN IA.ACCOUNT_ID IS NOT NULL<br>             AND FA.ACCT_TYPE = 'I'<br>             AND EA.ACCOUNT_ID IS NULL<br>            THEN 'Valid - In-House Only'<br>        ELSE 'Violation'<br>    END AS RULE_7_CHECK<br>FROM PAYMENT AS P<br>LEFT JOIN INHOUSE_ACCOUNT AS IA<br>    ON IA.ACCOUNT_ID = P.ACCOUNT_ID<br>LEFT JOIN FINANCING_ACCOUNT AS FA<br>    ON FA.ACCOUNT_ID = IA.ACCOUNT_ID<br>LEFT JOIN EXTERNAL_ACCOUNT AS EA<br>    ON EA.ACCOUNT_ID = P.ACCOUNT_ID<br>ORDER BY P.PAYMENT_ID;</code></pre> |

Alias mapping: P = PAYMENT; IA = INHOUSE_ACCOUNT; FA = FINANCING_ACCOUNT; EA = EXTERNAL_ACCOUNT.

## Alias pattern summary

| Full table name | Common alias |
|---|---|
| CUSTOMER | C |
| VEHICLE | V |
| SALE | S |
| FINANCING_ACCOUNT | FA |
| INHOUSE_ACCOUNT | IA |
| EXTERNAL_ACCOUNT | EA |
| EXTERNAL_LENDER | L |
| PAYMENT | P |

Aliases are local to a query. The database does not permanently store these short names.
