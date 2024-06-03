1. Giới thiệu<br> 
- Dự án cá nhận về Khám phá Dữ liệu (EDA) và trực quan hóa về một nền tảng thương mại điện tử tại Brazil. Dự án cá nhận được xử lý hoàn toàn bằng Power BI.

2. Các kĩ năng được sử dụng
- Data Cleaning/Validation in Power Query
- Data Modelling
- Data Visualization
- DAX: Calculated Measures, Calculated Columns.
- Filters and slicers
- Drill-through

3. Tập dữ liệu của dự án <br>
- Bộ dữ liệu bao gồm 9 bảng: Olist_customers, Olist_geolocation, Olist_order_items, Olist_order_payments, Olist_order_reviews, Olist_orders, Olist_products, Olist_sellers, Product_category_name.
- Chi tiết mô tả xem tại [ĐÂY](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Description_dataset.pdf)

4. Business_question <br>
- Tổng quan doanh thu
  - Tổng số doanh thu và đơn đặt hàng theo thời gian
  - Các danh mục sản phẩm được bán chạy nhất theo số lượng và doanh thu
  - Danh mục sản phẩm có lợi nhuận gộp cao nhất <br>
- Đơn hàng và thanh toán
  - Giá trị trung bình của đơn hàng theo danh mục sản phẩm 
  - Đánh giá mối liên hệ giữa phương thức thanh toán và danh mục sản phẩm/địa lý <br>
- Phân tích mối quan hệ
  - Đánh giá mối liên hệ giữa điểm đánh giá sản phẩm trung bình và hiệu suất bán hàng 
  - Đánh giá mối liên hệ giữa điểm đánh giá sản phẩm trung bình và hiệu suất sản phẩm
  - Đánh giá mối liên hệ giữa tỷ lệ hủy đơn trung bình và hiệu suất của người bán 
  - Đánh giá mối liên hệ giữa điểm đánh giá sản phẩm trung bình và tổng doanh tu<br>
- Khách hàng và vị trí địa lý
 
  - Vị trí địa lý có lượng khách hàng lớn và tỷ lệ khách hàng mua lại hàng 

5. Làm sạch dữ liệu
- Tập dữ liệu được giải nén, làm sạch và import vào Power BI. Chi tiết các bước xem tại ĐÂY.
	
6. Mô hình dữ liệu
- Dữ liệu sau khi được chuẩn hoá được thêm vào lược đồ hình sao (Star schema). Xem chi tiết ở hình ảnh dưới
- Chi tiết các bảng:
  - Fact table: Olist_Order_items: Chứa các số liệu định lượng cho đơn hàng
  - Factless table: Olist_Orders : Chứa ngày để theo dõi trạng thái bán hàng/mua hàng và không có bất kỳ số liệu
  - Dimesion table: list_customers, Olist_geolocation, Olist_order_payments, Olist_order_reviews, Olist_products, Olist_sellers, Date_Table (bảng mới được tạo)

![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Data%20Model.png)

7. Phân tích dữ liệu với Power BI <br>

*Q1. Tổng số doanh thu và đơn đặt hàng theo thời gian
```
Total Revenue = 
CALCULATE(
    SUM(olist_order_payments_dataset[payment_value]),
      olist_orders_dataset[order_status] = "delivered"
)
```
```
Total Orders = COUNT(olist_orders_dataset[order_id])
```
- Doanh thu cho các đơn hàng giao thành công từ 9/2016 - 8/2019 là R$15,422,461.77.
- Doanh thu tăng liên tục trong 3 năm, đặc biệt tăng trưởng mạnh mẽ vào giữa năm 2016 và 2017. So sánh doanh thu theo tháng thấy được doanh thu sụt giảm mạnh vào tháng 9, do chỉ có số liệu bán hàng từ 10/2026 - 8/2018 và chỉ có duy nhất số liệu bán hàng tháng 9/2019. Xét doanh thu theo ngày thấy sự tăng đột ngột về đơn hàng và doanh thu vào thứ 6 ngày 24/11/2017 do có khuyến mại lớn của ngày Back Friday
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Total_revenue_by_time.png)

- Số lượng đơn hàng đã giao thành công là 96.478 trên tổng số 99.941 đơn hàng đã được đặt. Tương tự như doanh thu bán hàng, ngày 24/11/2017 cũng có số lượng đơn đặt hàng cao nhất.
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Total_order_by_time.png)

*Q2. Các danh mục sản phẩm được bán chạy nhất theo số lượng và doanh thu
- Phân tích mức độ về mức độ phổ biến theo top 20, danh mục sản phẩm được ưa chuộng nhất là Bed_Bath_Table có 9.417 đơn đặt hàng, tiếp theo Health_Beauty (8.836) và Sports_Leisure (7.720). Tuy nhiên Health_Beauty có doanh thu bán hàng cao nhất là R$1.419.509,89, tiếp theo là Watches_Gifts category (R$1,269,684.96) và Bed_Bath_Table (R$1,249,411.56). Điều này chứng tỏ sản phẩm được bán chạy nhất không đồng nghĩa với mang lại doanh thu lớn nhất
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Product_popularity.png)

*Q3. Danh mục sản phẩm có lợi nhuận gộp cao nhất <br>
- Vì tập dữ liệu có sẵn không có giá vốn, do đó chúng ta không thể tính toán biên lợi nhuận, chỉ có thể kiểm tra biên lợi nhuận gộp. Công thức DAX tính toán biên lợi nhuận gộp:
```
Gross_Profit_Margin = DIVIDE(
                         CALCULATE(SUM('olist_order_items'[price]),
                            olist_orders[order_status]="delivered"),
                               [Total Revenue])
```
- Sản phẩm có biên độ lợi nhuận gộp cao nhất là Computers với 95.71%, tiếp the là Small_Appliances_Home_Oven_And_Coffee (94.56%), Portable_kitchen_and_food_preparators (93.04%).
  ![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Profit_margin.png)

*Q4. Đánh giá mối liên hệ giữa giá trị trung bình của đơn hàng theo danh mục sản phẩm và phương thức thanh toán <br>
- Từ Q2 ta thấy không phải sản phẩm phổ biến nhất sẽ mang lại doanh thu cao nhất, chúng ta sẽ đi sâu hơn bằng cách đánh giá qua giá trị trung bình (AOV) của đơn hàng và mối liên hệ của AOV với danh mục sản phẩm cũng như là phương thức thanh toán. Công thức DAX tính toán AOV:

```
Average Order Value = 
                  DIVIDE ([Total Revenue], 
                      CALCULATE(COUNTROWS('olist_orders_dataset'), 
                         olist_orders_dataset[order_status] IN {"Delivered"})
)
```
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/AOV.png)
- Giá trị đơn hàng trung bình là R$159.85. Xét theo các danh mục sản phẩm và loại thanh toán, chúng ta thấy rằng danh mục sản phẩm có giá trị đơn hàng trung bình cao nhất là Computers, trong khi Credit card là loại thanh toán có giá trị đơn hàng trung bình cao nhất. Điều này phản ánh sự ưa thích và sự tin tưởng của khách hàng vào những loại sản phẩm và phương thức thanh toán này, hoặc có thể là kết quả của các chính sách giảm giá, ưu đãi đặc biệt hoặc khuyến mãi đối với những mặt hàng hoặc phương thức thanh toán này.
  
*Q5. Đánh giá mối liên hệ giữa phương thức thanh toán và danh mục sản phẩm/địa lý <br>
- Tử Q4 ta thấy phương thức thanh toán Credit Card là loại thanh toán có giá trị đơn hàng trung bình cao nhất. Vậy liệu phương thức thanh toán này có phổ biến với toàn bộ các đơn hàng không?

![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Payment_popularity.png)

![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Payment_type%20by.%20png.png)
- Từ các biểu đồ tên ta thấy được:
  - Credit Card là phương thức thanh toán phổ biến nhất, ít phổ biến nhất là Debit Card.
  - Credit Card và Bolero đều được sử dụng để mua 72 danh mục sản phẩm.
  - Các bang sử dụng phương thức Credit Card để mua hàng nhiều nhất là Minas Gerais, Rio de Janeiro, và Sao Paulo. Điều này phản ánh sự phổ biến của hình thức thanh toán này ở các vùng đô thị lớn của Brazil.

*Q6. Đánh giá mối liên hệ giữa điểm đánh giá sản phẩm trung bình và hiệu suất bán hàng 
- Để đánh giá được hiệu suất bán hàng, sử dụng tổng doanh thu của seller và rating của seller. Công thức DAX tính rating của seller:
```
Average Rating = AVERAGE('olist_order_reviews_dataset'[review_score])
```
![Ảnh]([https://github.com/namnguyen2910/DA_Olist_Store](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/seller%20rating%20-%20sale%20performance.png))

6: What is the distribution of seller ratings on Olist, and how does this impact sales performance?
Now we know how many of the Olist sellers are active, we will now evaluate how customers rate them and the impact on their performance. To do this, I created a calculated measure to get the Average customer rating from the review scores. The DAX measure is shown below:

Average Rating = AVERAGE('olist_order_reviews'[review_score])
The sales performance was then evaluated using the total revenue per seller and average rating. The resulting visual shows that Sellers with high ratings tend to have higher sales outcome than those with lower ratings, with the highest performing seller having an average rating of 4.12.



7: How many customers have made repeat purchases on Olist, and what percentage of total sales do they account for?
Based on the previous visual, we observed that more than 50% sellers were rated fairly high. We now want to see if the seeming satisfaction led to repeat purchases by the customers. To determine this, I created a calculated column to filter out orders that are repeat purchases based on the customers’ unique id. The DAX formula below was used to create the column:

Repeat_Purchases = CALCULATE(
                      IF(COUNT('olist_orders'[order_id])>1,1,0),
                          ALLEXCEPT(olist_customers,olist_customers[customer_unique_id]))
The formula calculates whether a customer has made more than one purchase or not by checking if the count of 'order_id' in the olist_orders table is greater than 1. If it is, it returns 1, indicating that the customer has made repeat purchases. Otherwise, it returns 0, indicating that the customer has made only one purchase. Afterwards I created a calculated measure to get the total count of customers that had repeat purchases i.e. customers whose count of 'order_id' in the olist_orders table returned 1. The DAX measure is shown below:

Repeat Purchase Customers = CALCULATE( 
                                DISTINCTCOUNT(olist_customers[customer_unique_id]),
                                   olist_customers[Repeat_Purchases] = 1,
                                       olist_orders[order_status] = "Delivered")
The measure calculates the distinct count of customers who have made repeat purchases and have their order status as ‘delivered’. Delivered orders alone were taken into account, as they represent true repeat purchases. Our computation output shows that 2,979 customers made repeat purchases.



Let’s now find out the impact of this repeat purchases on the Total revenue.



Based on the visual above we can see that repeat purchases accounted for 5.84% (R$900,727.35) of the total revenue.

8: What is the average customer rating for products sold on Olist, and how does this impact sales performance?
From the exploration so far, we see that less than 5% of the customers made repeat purchase. To ascertain the possible cause of this behaviour, let’s evaluate the impact of customer ratings for the products on the sales performance. Average rating was computed to be 4.07, which is a good rating on a scale of 1 to 5.



How does this impact the sales performance?



Based on the resulting visual shown above, the product with the highest sales amount has a rating of 4.21, which is above the overall average rating.

9: What is the average order cancellation rate on Olist, and how does this impact seller performance?
As we continue to evaluate Olist customers’ behaviour, from repeat purchases to ratings for sellers/products, we will now look into cancelled orders, to understand its effect on the performance of sellers/merchants on the Olist platform. It will also be important to know factors that can influence these order cancellations and seller performance. To explore this stage, I computed the average order cancellation rate using the measure below and its result gave 0.63%.

Average Order Cancellation Rate = 
                                 DIVIDE(
                                    CALCULATE(COUNTROWS(olist_orders),
    	                                  olist_orders[order_status] = "canceled"),
                                          COUNTROWS(olist_orders))


I then evaluated average order cancellation rate for the sellers based on Minimum price of product, Total revenue, average rating, and number of orders. I leveraged AI feature of Power BI for this by using the Key Influencers visual. Based on the visual below, we can opine that as average rating decreases, average order cancellation rate increases and vice versa. Similarly, as minimum price of a seller decreases, it becomes probable that the average order cancellation rate will decrease and vice versa.



Furthermore, an increase in average order cancellation rate, results in decrease in number of orders sold and Total revenue, while a decrease in average order cancellation rate, gives rise to an increase in number of orders sold and Total revenue.



There is therefore need to mitigate the factors that may likely increase average order cancellation rate.

10: What are the top-selling products on Olist, and how have their sales trends changed over time?
Having understood some aspect of Olist customers’ behaviour, let’s explore the products that are advertised on the platform for sale. I’ll be looking at the top 10 selling products and their sales trend over time. Using the decomposition tree visual below, I checked the product categories over the years in terms of their sales (total revenue).



From the visual above, we can see that across the years, no product category remained the topmost selling product – In 2016, it was Furniture_Decor, in 2017 – Bed_Bath_Table took the lead, and in 2018, Health_Beauty became the topmost category. However, we also observed that the Health_Beauty category was constantly among the top 3 selling product categories across the 3year period in view.

11: Which payment methods are most commonly used by Olist customers, and how does this vary by product category or geographic region?
We will now move on to evaluate how customers make payments for the purchased products.



From the visual above we see that most common payment method is the Use of Credit card and the least method is the use of debit card. Exploring further across the locations and products in the visual below, we see the same pattern of the most used payment method being Credit card, followed by Boleto. However, we also observed that both Credit card and Boleto were used within all the 72 product categories.



12: How do customer reviews and ratings affect sales and product performance on Olist?
Based on response to similar questions, Q6 and Q8, we have seen that sellers and products with the highest revenue/sales performance have ratings above average (4.07).



Exploring further, we observe from the visual above, that products with average review score of “2.0” had lesser revenue than products with average review score of “1.0”. This can be due to the quantity of the products sold that fell within the 1.0 rating, which is about 3 times that of the ones within the 2.0 rating. Hence, we can say that as rating/review score increases, the total revenue increases.

13: Which product categories have the highest profit margins on Olist, and how can the company increase profitability across different categories?
Olist is not a non-profit organisation, hence it is important to know products that are more profitable for the business. We get this by evaluating the profit margin on the platform across the categories. To calculate the profit margin, we need to get the net profit by subtracting the cost price from the selling price(price), divide by the total revenue and multiply by 100 (percent). The data available does not capture the cost price, hence we are only able to check for the gross profit margin. The DAX measure below was used to calculate the gross profit margin.

Gross_Profit_Margin = DIVIDE(
                         CALCULATE(SUM('olist_order_items'[price]),
                            olist_orders[order_status]="delivered"),
                               [Total Revenue])
The resulting Gross Profit Margin computed is 85.73%.



Exploring across the categories, we observe that the product category with the highest gross profit margin is "Computers" with 95.71%, followed by the “Small_Appliances_Home_Oven_And_Coffee" category (94.56%), and then “Portable_kitchen_and_food_preparators" category (93.04%). Visual is shown below.



To increase profitability across different categories, Olist platform can Collaborate/Strengthen relationships with sellers and negotiate better terms such as bulk discounts, exclusive deals, etc. They can also work with the sellers to improve their pricing strategy, ensuring that the pricing of their products are competitive, but not so low as to lose revenue. They may also consider using tiered pricing, where different prices are charged for the same product depending on the quantity or other factors.

Secondly, they can use strategies that encourage customers to purchase additional products or upgrade their purchases by implementing cross-selling and upselling techniques. To do this, they can leverage Artificial intelligence (AI) technology and data analytics to offer personalized recommendations on the platform based on customer preferences and buying patterns. This can increase the average order value and contribute to higher profitability. As we can see from our analysis that the category with the highest profit margin also had the highest Average order value.



Additionally, they can consider phasing out or re-evaluating low-margin products that are not contributing significantly to profitability.

14: How does Olist's marketing spend and channel mix impact sales and customer acquisition costs, and how can the company optimize its marketing strategy to increase ROI?
Marketing Spend refers to the amount of money allocated to marketing activities, which directly influences the reach and impact of Olist's promotional efforts.

Channel mix, on the other hand, refers to the distribution of marketing efforts across different mediums/channels such as social media, email marketing, Partnership with Influencers, etc. The more diverse the channels mix, the more likely it will be to reach wider audience and potential leads that can be converted to customers. This will definitely have impact on the sales outcome.

Therefore, Increasing marketing spend by having sufficient allocation of funds to diverse channel mix can potentially increase brand visibility, encourage new customer acquisition, and ultimately drive sales. However, it is pertinent that the business prioritizes channels with high leads conversion rates, while striking a balance between the marketing budget and the return on investment (ROI) generated.

On Customer Acquisition Costs (CAC), which is the expenses incurred in acquiring a new customer, Olist can evaluate the cost-effectiveness of each acquisition channel by assessing the cost per lead, cost per customer, and customer lifetime value (CLTV) to determine the efficiency of marketing efforts. If the CAC is too high, adjustments can be made to optimize the strategy and reduce acquisition costs.

To optimize its marketing strategy and increase ROI, Olist can consider leveraging data analytics to gain insights into customer behavior, preferences, and purchasing patterns. Such data-driven decision making will enable Olist to identify high-performing channels, adjust the channel mix where applicable, and optimize resource allocation.

Secondly, Olist can implement targeted marketing campaigns, where customer demographics, interests, and buying habits are analysed, and then, the messaging and promotional offers are tailored to meet specific customer segments. This can improve customer engagement, increase conversion of leads to Customer, and boost sales.

15: Geolocation having high customer density. Calculate customer retention rate according to geolocations
Finally, we will evaluate the spread of Olist customer base in terms of location. There were 99,441 customers for the 3-year period in review. In terms of high Customer density, the top three locations are Sao Paulo, Rio de Janeiro, and Minas Gerais



But does High Customer density equate high Customer retention rate? To check for this, the overall Customer retention rate was computed using the DAX measure below. The results was 3.0

Customer Retention Rate % = 
                          DIVIDE('olist_customers'[Repeat_purchase_customers],
                            COUNTROWS('olist_customers')) 
                              * 100
Exploring further, we see that in terms of customer retention, we have the location with the highest Customer retention rate to be Acre, despite being among the last 3 location with the least customer density. The next two high customer retention locations are Rondonia and Mato Grosso.



Key Insights
‌Olist E-commerce Store generated a total revenue of 15.42 million Brazilian real(R$15,422,461.77) within the 3-year period in review. The high revenue may be attributed to presence of a wide range of product categories, competitive pricing, a user-friendly platform that attracts and retains customers, etc.

In 2017, ‌16.66% of the total revenue was generated in November, with the Black Friday sales (R$175,250.94) accounting for 15.19% of the total revenue generated in November 2017 (R$1,153,528.05). The popularity of the "Black Friday" sales in Brazil will have enhanced this outcome.

‌The platform recorded order delivery success rate of 97% for all placed orders.This is a pointer that Olist has a well-established logistics and Order fulfillment system in place, which ensures that customers receive their orders reliably and on time.

‌The products categories with the highest Quantity of orders didn't always generate higher sales volume. This may be due to the price and quality of the products.

‌The Average order value (AOV) for the Olist platform is R$159.85 . This high AOV suggests that customers are buying more expensive products.

The Platform has ‌96% active sellers, which suggests that Olist provides a favorable platform for merchants/sellers to conduct their business, resulting in a consistent increase in the number of active sellers over time.

Customers that made repeat purchases generated ‌5.84% of the total revenue. The presence of repeat purchases indicates that Olist has a loyal customer base.

‌Credit Cards are the major type of payment used by Olist Customers, followed by Boletos while the least used method is Debit card. This may be due to credit cards being more widely accepted than debit cards. This preferrence may also be due to the convenience and security offered by credit card payments.

‌Products and Sellers with the highest sales performance had ratings that were above average(4.07). However, having the highest rating band of 5, didn't give rise to high sales performance for most products and sellers. This could be an indicator of Customer preferance for low priced goods.

The Average Order Cancellation rate is 0.63% - which can be indicative of high Customer Satisfaction. However, the Olist e-commerce platform lost R$143,255.60 due to cancelled Orders. Customer ratings and minimum price of Sellers' products were found to be key influencers of the Average Order Cancellation rate, which in turn impacts the number of orders placed and revenue generated.

‌The product categories' sales trend are competitive as a new category always takes the lead as "Top selling product" each year.

The Gross profit margin is 85.73%. The product category, Computers, which has the highest order value also generated the highest Gross profit margin. This is likely due to higher-priced products or better profit margins on items in the category.

‌Geolocation with the highest Customer density is Sao Paulo while the Geolocation with the highest Customer retention rate is Acre. However, the top 3 geolocations with high Customer densities also had retention rates above average(3.0).

Recommendations:
Olist should implement a seasonal/quarterly promotional events like the "Black Friday" sales to drive customer engagement, high product purchase and increased revenue.

They should strive to maintain high product quality and competitive pricing in order to minimize cancellations, maximize customer satisfaction, and boost revenue.

Loyalty programs such as special discounts for repeat customers and free shipping to targeted locations like Sao Paulo and Acre can be implemented to drive increase in repeat purchases, increase in customer retention, and acquisition of new Customers.

The high revenue & high profit margin suggests that the business has established a profitable online venture. They should consider investing in new products and services, expanding into new markets. This will drive new customer acquisition and ensure Olist stays ahead in the E-commerce business in Brazil.

Conclusion
By responding to the business questions, I have achieved the goal of helping Olist gain better insights into their e-commerce platform and know how to optimize available opportunities for growth.

Also, working on this dataset expounded my knowledge on e-commerce business metrics, and helped me improve my skills in Power BI DAX and Data modelling. I hope to build a dashboard from this report and provide an accessible link for interaction with the visuals soon.

Your candid comments, constructive criticisms and feedbacks are anticipated.
