SELECT 
    MONTH, 
    ITEM_DESCRIPTION, 
    COUNT(*) AS product_count
FROM 
    sales
GROUP BY 
    MONTH, ITEM_DESCRIPTION
ORDER BY 
    MONTH, ITEM_DESCRIPTION;

SELECT 
    SUPPLIER
FROM 
    sales
WHERE 
    ITEM_TYPE = 'WINE'  -- Lọc sản phẩm 'WINE'
GROUP BY 
    SUPPLIER;  -- Nhóm theo công ty để liệt kê các công ty sản xuất 'rượu A'

WITH RankedSales AS (
    SELECT 
        SUPPLIER, 
        ITEM_TYPE, 
        MONTH,
        COUNT(*) AS product_count,
        ROW_NUMBER() OVER (PARTITION BY MONTH ORDER BY COUNT(*) DESC) AS rank
    FROM 
        sales
    WHERE 
        MONTH IN (1, 2, 3)
    GROUP BY 
        SUPPLIER, ITEM_TYPE, MONTH
)
SELECT 
    SUPPLIER, 
    ITEM_TYPE, 
    MONTH, 
    product_count
FROM 
    RankedSales
WHERE 
    rank <= 3  -- Lấy top 3 công ty trong mỗi tháng
ORDER BY 
    MONTH, product_count DESC;

SELECT 
    COUNT(DISTINCT ITEM_TYPE) AS company_count
FROM 
    sales
WHERE 
    ITEM_TYPE = 'WINE';



