Inner Join - inside of a venn diagram

```
SELECT TOP (1000) 
	product_name,
	model_year,
	brand_name,
	category_name
  FROM [BikeStores].[production].[products] p
  INNER JOIN [BikeStores].[production].[brands] b
  ON p.brand_id = b.brand_id
  INNER JOIN [BikeStores].[production].[categories] c
  ON p.category_id = c.category_id
```