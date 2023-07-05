# **Customer Segmentation and Recommendation System to Increase Customer Retention Olist (Brazilian E-Commerce Integrator)**

Created by    : AlphaGroup_JC_DS_OL_09 \
Team Members  :
- Irfan Zidni (zidniirfan155@gmail.com)
- Ali Yafi (ir.yafee@gmail.com)
- Noor Kharismawan Akbar (akbar.noorkharismawan@gmail.com)

Ada berbagai marketplace di brazil seperti americanas.com, submarino, amazon dll yang mana marketplace tersebut terintegrasi dengan eCommerce platform, yaitu **Olist**, yang memberikan kemudahan kepada seller untuk menjual produknya secara online dengan cara mempromosikan hingga ke 13 eCommerce marketplace di Brazil.

**Customer Retention di Brazil E-Commerce sangat rendah**, pada rentang 2016-2018 hampir 97% Customer tidak melakukan pembelian kedua. Masalah ini dapat muncul karena tidak adanya strategi marketing untuk menjaga retensi customer yang efektif serta kurangnya pemahaman mengenai perilaku pelanggan (segmentasi).

Olist ingin mengetahui **karakteristik customer** di setiap segmentnya sehingga bisa memberikan strategi marketing yang tepat untuk meningkatkan customer retention dengan menggunakan analisis pemasaran berbasis perilaku pelanggan yang disebut **RFM (Recency, Frequency, and Monetary)**. Metode ini efektif untuk mengetahui karakteristik pelanggan berdasarkan perilaku pembelian mereka. Dengan RFM, kita akan mendapatkan nilai yang diinginkan dan fitur-fitur tersebut akan digunakan sebagai input dalam **K-means**, untuk menentukan kemiripan (clustering / segmentation). Kita akan membantu Team Marketing Olist dalam memutuskan produk apa yang akan di rekomendasikan kepada pelanggan sesuai dengan karakteristik dari pelanggan tersebut menggunakan **Recommendation System**.

Dataset: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_products_dataset.csv

Ada beberapa **anomali data** seperti duplikat saat melakukan merge dataset, typo, dan Missing value sehingga kita lakukan handling seperti menghapus duplikat data sebelum merge dataset, mengganti value yang typo dan mengisi missing value berdasarkan domain knowledge. Ada beberapa missing value yang tetap kita pertahankan untuk keperluan analisa selanjutnya.

**Performa** Olist Store dari tahun 2016 - 2018 secara umum **terus meningkat** setiap tahunnya, dimana jumlah order, Revenue dan customer yang melakukan order terus meningkat, digambarkan pada grafik di bawah ini

![image](https://github.com/PurwadhikaDev/AlphaGroup_JC_DS_OL_09_FinalProject/assets/113813837/2a43ac5e-2d9f-4f3d-839d-b34a9e39d96d)

Namun, sesuai dengan data yang peroleh bahwa **retensi pelanggan sangat rendah**, analisa terhadap retensi bisa dilihat menggunakan cohort analysis pada gambar di bawah ini

![cohort](https://github.com/PurwadhikaDev/AlphaGroup_JC_DS_OL_09_FinalProject/assets/113813837/a8199146-08ed-402a-80ea-09904fa27d87)

Clustering yang akan kita lakukan berdasarkan *behaviour* customer dalam melakukan transaksi, sehingga kita memerlukan fitur RFM sebagai fitur utama dalam melakukan clustering.
- **Recency**: Mengukur kapan terakhir suatu customer berbelanja produk kita
- **Frequency**: Mengukur seberapa sering customer membeli produk kita
- **Monetary**: Mengukur seberapa banyak customer mengeluarkan spent terhadap produk kita.

![rfm](https://github.com/PurwadhikaDev/AlphaGroup_JC_DS_OL_09_FinalProject/assets/113813837/d0a90b2a-3ef3-4b15-aca7-6f67503dd720)

Gambar diatas menggambarkan *behaviour* customer berbelanja, dari histori transaksi customer di olist kita bisa melihat bahwa **mayoritas customer** order hanya 1x (Frequency), dengan Recency cukup bervariasi sampi 700 an hari artinya ada customer yang melakukan **transaksi 700 hari yang lalu**, dan monetary yang cukup bervariasi  dari R$ 0 - R$ 13.000 namun **paling banyak di bawah R$ 1000**.

RFM tersebut kita masukkan ke dalam K-Means Clustering dimana kita melakukan **beberapa experiment** untuk mencari cluster terbaik, seperti penggunaan transformasi logaritmic pada data, penggunaan modul Winsorizer, scaling menggunakan MinMaxScaler, RobustScaler, StandarScaler dengan beberapa evaluation metric seperti Elbow method, Silhoutte Score, Snake Plot, dan Flattened Graph.

Model terbaik yang dapat melakukan cluster dengan menggunakan **Winsorizer** dengan evaluation metric menggunakan **Snake Plot**. Model ini dapat **memisahkan** antara customer yang berbelanja 1x dan lebih dari 1x sehingga ini sesuai dengan bisnis problem kita mengenai customer retention. Sehingga, Cluster yang terbentuk sebanyak 5.

![clustering](https://github.com/PurwadhikaDev/AlphaGroup_JC_DS_OL_09_FinalProject/assets/113813837/03b66af3-07cf-4274-9478-c83ced46da8d)

Karakteristik Cluster yang terbentuk sebagai berikut:

| No Cluster | Nama Cluster | Analisis RFM | Analisis Waktu Buy | Analisis Produk |
| -- | -- | -- | -- | -- |
| 0 | ROYAL | Sama seperti namanya, customer ini merupakan customer yang cukup royal. dimana dia membelanjakan banyak uang untuk **OLIST STORE** dalam 1x pembelian dengan nilai rata-rata R$ 333.04. Namun cluster ini sudah agak lama tidak berbelanja di **OLIST STORE**. | Customer ini cukup beragam waktu pembelian nya, namun didominasi pembelian di Quarter 1 & 2 dengan angka sekitar 60% | bed_bath_table, computers_accessories, sports_leisure |
| 1 | SUSTAIN | Customer yang masuk ke dalam segmen ini merupakan satu-satunya cluster yang pernah berbelanja di **OLIST STORE** lebih dari 1x. Dimana secara rata-rata mereka menyumbang pemasukan terbesar ke **OLIST STORE**, namun cluster ini bukan customer yang baru saja beli. | Waktu beli segmen ini cukup beragam dan tidak ada dominasi suatu Quarter | bed_bath_table, furniture_decor, sports_leisure |
| 2 | JEOPARDY | Customer ini merupakan customer yang sudah lama tidak berbelanja di **OLIST STORE** secara rata-rata mereka sudah lebih dari 1 tahun tidak berbelanja lagi, mereka juga hanya 1x berbelanja dengan nominal yang tergolong rendah dibandingkan cluster lain | Customer ini cukup beragam waktu pembelian nya, namun didominasi pembelian di Quarter 3 & 4 dengan angka sekitar 60% | bed_bath_table, computers_accessories, sports_leisure |
| 3 | FRESH | Customer segmen ini merupakan customer yang baru-baru ini berbelanja di **OLIST STORE**, namun mereka belum membelanjakan uang yang banyak ke **OLIST STORE** dan baru berbelanja 1x. | Customer segmen ini 75% berbelanja pada Quarter 3. dimana sisanya berbelanja di Quarter 2 | health_beauty, bed_bath_table, housewares |
| 4 | MODEST | Customer segmen ini bertipe hemat, dimana mereka berbelanja paling sedikit dibandingkan cluster lain. Mereka hanya 1x membeli dan merupakan segmen dengan resensi terbaik kedua di **OLIST STORE** | Pada Segmen ini cukup dominan membeli pada Quarter 1 & 2 dengan persentase 85% | health_beauty, bed_bath_table, computers_accessories |

Setelah dilakukan segementasi customer, kita mencoba membangun sistem rekomendasi. Tujuan dari dibuatnya sistem rekomendasi ini adalah untuk membantu Team Marketing OLIST untuk menentukan item apa saja yang cocok untuk direkomendasikan kepada pelanggan yang sudah disegmentasi sebelumnya. Recommendation Sytem yang dibangun dapat merekomendasikan item berdasarkan segmentnya disandingkan dengan opsi yang dapat dipilih seperti customer state.

