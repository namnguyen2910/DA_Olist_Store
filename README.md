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
  - Mối liên hệ giữa phương thức thanh toán và danh mục sản phẩm/địa lý <br>
- Phân tích mối quan hệ
  - Mối liên hệ giữa điểm đánh giá người bán trung bình và hiệu suất bán hàng
  - Mối liên hệ giữa tỷ lệ mua lại hàng và tổng doanh thu
  - Mối liên hệ giữa điểm đánh giá sản phẩm trung bình và hiệu suất bán hàng
  - Mối liên hệ giữa điểm đánh giá sản phẩm và doanh thu tương ứng
  - Vị trí địa lý có lượng khách hàng lớn và tỷ lệ khách hàng mua lại hàng 

5. Làm sạch dữ liệu
- Tập dữ liệu được giải nén, làm sạch và import vào Power BI. Chi tiết các bước xem tại ĐÂY.
	
6. Mô hình dữ liệu
- Dữ liệu sau khi được chuẩn hoá được thêm vào lược đồ hình sao (Star schema). Xem chi tiết ở hình ảnh dưới
- Chi tiết các bảng:
  - Fact table: Olist_Order_items: Chứa các số liệu định lượng cho đơn hàng
  - Factless table: Olist_Orders : Chứa ngày để theo dõi trạng thái bán hàng/mua hàng và không có bất kỳ số liệu
  - Dimesion table: list_customers, Olist_geolocation, Olist_order_payments, Olist_order_reviews, Olist_products, Olist_sellers, Dimdate (bảng mới được tạo)

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
- Sản phẩm có biên độ lợi nhuận gộp cao nhất là Computers với 95.71%, tiếp theo là Small_Appliances_Home_Oven_And_Coffee (94.56%), Portable_kitchen_and_food_preparators (93.04%).
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

*Q6. Đánh giá mối liên hệ giữa điểm đánh giá trung bình của người bán và hiệu suất bán hàng 
- Để đánh giá được hiệu suất bán hàng, sử dụng tổng doanh thu của người bán và rating của người bán. Công thức DAX tính rating của người bán:
```
Average Rating = AVERAGE('olist_order_reviews_dataset'[review_score])
```
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/seller%20rating%20-%20sale%20performance.png)
- Kết quả cho thấy rằng những người bán có đánh giá cao có xu hướng đạt được kết quả bán hàng cao hơn so với những người có đánh giá thấp hơn, với người bán hàng có hiệu suất cao nhất có đánh giá trung bình là 4,12.

*Q7. Mối liên hệ giữa tỷ lệ mua lại hàng và tổng doanh thu
- Từ Q6 ta thấy được có trên 50% tỷ lệ người bán được đánh giá khá cao. Vậy tỷ lệ người mua lại hàng chiếm bao nhiêu % và đóng góp vào tổng doanh thu là bao nhiêu? Trước hết tạo ra 1 cột để lọc các đơn hàng được tính là đơn hàng mua lại:
```
Repeat_Purchases = CALCULATE(
                      IF(COUNT('olist_orders_dataset'[order_id])>1,1,0),
                          ALLEXCEPT(olist_customers_dataset,olist_customers_dataset[customer_unique_id]))
```
- Nếu có 1 khách hàng có order_id >1 thì nó trả về 1, chỉ ra rằng khách hàng đã thực hiện mua hàng lặp lại. Ngược lại, nó trả về 0, chỉ ra rằng khách hàng chỉ đã thực hiện một đơn hàng. Sau đó, tôi đã tạo một biểu thức tính để có được tổng số lượng khách hàng đã mua hàng lặp lại, tức là các khách hàng mà số lượng 'order_id' trong bảng olist_orders trả về 1. Sau đó tính tổng lượng khách hàng là khách hàng mua lại bằng công thức DAX :
```
Repeat Purchase Customers = CALCULATE( 
                                DISTINCTCOUNT(olist_customers_dataset[customer_unique_id]),
                                   olist_orders_dataset[Repeat_Purchases] = 1,
                                       olist_orders_dataset[order_status] = "Delivered")
```
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Repeat%20Purchase.png)
- Tỷ lệ mua hàng trở lại ~5.84% (R$900,727.35).

*Q8. Mối liên hệ giữa điểm đánh giá sản phẩm trung bình và hiệu suất bán hàng
- Từ Q7 ta thấy được tỷ lệ người mua hàng trở lại không cao, vậy có phải do chất lượng sản phẩm không tốt? Bằng cách lọc ra các sản phẩm không có review score, Công thức DAX dưới đây sẽ tính được rating của sản phẩm:
```
ProductRating = 
AVERAGEX(
    FILTER(
        RELATEDTABLE('olist_order_reviews_dataset'),
        'olist_order_reviews_dataset'[review_score] <> BLANK()
    ),
    'olist_order_reviews_dataset'[review_score]
)
```
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/product%20rating%20-%20sale%20performance.png)
- Điểm trung bình sản phẩm là 4.07, và sản phẩm đạt doanh thu lớn nhất có điểm là 4.21. Với điểm đánh giá cao, sản phẩm có khả năng thu hút và giữ chân khách hàng tốt hơn từ đó làm tăng doanh thu bán hàng.

*Q9. Mối liên hệ giữa điểm đánh giá sản phẩm và doanh thu tương ứng
- Từ Q6/Q8, ta thấy sản phẩm/người bán có doanh thu cao nhất đèu có điểm trung bình >4. Vậy có phải điểm trung bình cao đồng nghĩa với doanh thu cao không?
  ![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/customer%20rating%20-%20order%20-%20revenue.png)
- Biểu đồ cho ta thấy được  các sản phẩm có điểm đánh giá là 2.0 có doanh thu ít hơn so với các sản phẩm có điểm đánh giá là 1.0. Mặt khác số lượng sản phẩm bán ra có điểm đánh giá 1.0 nhiều hơn khoảng 3 lần so với số lượng sản phẩm có điểm đánh giá 2.0. Do đó, chúng ta có thể nói rằng khi điểm đánh giá tăng, tổng doanh thu cũng tăng.

*Q10. Mối liên hệ giữa mật độ khách hàng theo vị trí địa lý và tỷ lệ giữ chân khách hàng
- Mật độ khách hàng theo địa lý:
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Customer%20Density.png)
  - Biểu đồ cho thấy mật độ khách hàng cao nhất là Sao Paulo, Rio de Janeiro và Minas Gerais.
- Để làm rõ hơn việc mật độ khách hàng cao có đồng nghĩa với tỷ lệ giữ chân khách hang hay không, ta sẽ tính tỷ lệ giữ chân khách hàng bằng DAX:
```
Customer Retention Rate (%) = 
                          DIVIDE('olist_orders_dataset'[Repeat Purchase Customers],
                            COUNTROWS('olist_customers_dataset')) 
                              * 100
```
![Ảnh](https://github.com/namnguyen2910/DA_Olist_Store/blob/main/Rentention.png)
  - Biểu đồ cho thấy tỉ lệ giữ chân khách hàng, vị trí có tỷ lệ cao nhất là Acre, mặc dù nằm trong ba vị trí cuối cùng với mật độ khách hàng ít nhất. Hai vị trí giữ chân khách hàng cao tiếp theo là Rondonia và Mato Grosso.

8. Insights
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

