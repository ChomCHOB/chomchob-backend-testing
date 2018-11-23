# chomchob-backend-testing

โจทย์จะมี 2 Part คือ
1. [Programming](#programming)
2. [Database](#database)

---

# Note
- มีเวลาทำโจทย์ 7 วันหลังจากได้รับ email 

---

# Programming

  ### Simple Wallet Api

  **Description**
  
  You need to create simple wallet api for transfer cryptocurrency between user A to user B. Also have admin ability to manage both cryptocurrency and exchange rate.

  **Requirement**

  - Admin can increase and decrease user cryptocurrency balance.
  - Admin can see all total balance of all cryptocurrency.
  - Admin can add other cryptocurrency such XRP, EOS, XLM to wallet.
  - Admin can manage exchange rate between cryptocurrency.
  - User can transfer cryptocurrency to other.
  - User can transfer cryptocurrency to other with difference currency such ETH to BTC with exchange rate.
  > Example
  >
  > User A transfer 1000 ETH to User B with exchange rate ETH/BTC equal to 0.05 so UserB will recieve 50 BTC
  - It ok whether cryptocurrency have decimal or not.

  **Technical Detail**
  - This API need to be written with Nodejs.
  - You can use any nodejs web framework but we prefer [express](https://expressjs.com/).
  - You can use any tool or library to help you build API. 
  - Database we prefer mariadb, but if you think other database is suitable it fine to use that db.
  - This is **not** decentralized wallet so no need to worry about blockchain stuff.
  
  **Bonus**
  - TDD
  - Deploy to cloud such as aws or gcp.


  > - ![Alt text](https://fs.chomchob.com/file/image?path=/admin/upload/2018-06-08/dfd301fb-a2e7-43b1-addb-d119ac2a3e02)
  


---

# Database

สมมติสถานการณ์ว่า 
คุณได้รับมอบหมายให้ออกแบบ Database ของระบบขาย code item สำหรับเกมต่างๆ 
ซึ่งคอยให้บริการแก่ลูกค้าที่ต้องการเข้ามาซื้อ code ไปเติมในเกม

  **โดยมีรายละเอียดดังนี้**
  
  - item ที่ขายจะต้องมี ชื่อสินค้า, รายละเอียดสินค้า, ราคาขาย, วันที่เปิดขาย, วันที่เลิกขาย
  - เมื่อลูกค้าซื้อ Item แล้วจะได้รับเป็น code (โดย code อาจถูกบันทึกไว้ล่วงหน้า หรือ อาจถูกสร้างหลังจากซื้อ ก็ได้)
  - item สามารถจัดโปรโมชั่นลดราคาในช่วงเวลาที่กำหนดได้ เช่น ปกติ ราคา 150 บาท จัดโปรเดือนมกราคม ลดราคาเป็น 100 บาท

  **Bonus**

  - item เป็นแบบ shared stock (ไม่ว่าซื้อ item ในราคาไหนจะต้องใช้ stock ของ code ที่เดียวกัน) เช่น มี ไอเท็ม A และ ไอเท็ม B อยู่ในระบบ หากมีลูกค้ามาทำการซื้อ A หรือ B ก็ตาม code ที่ลูกค้าได้รับ จะถูกดึงมาจาก stock เดียวกัน
  - item อาจถูกขายแบบ Bundle เช่น ขาย สกินตัวละครพร้อมกันสองตัวในราคาพิเศษ  หรือขาย กล่องสุ่มไอเท็ม 5 กล่อง ในราคาถูกกว่าปกติ

  **Hint**

  - สร้าง database ในรูปแบบของ model จาก ["sequelize"](https://github.com/sequelize/sequelize)

  
  **Example Model**

  ```js
  const User = sequelize.define('User', {
    id: Sequelize.INTEGER,
    name: Sequelize.STRING,
    age: Sequelize.INTEGER,
  },{
    classMethods: {
      associate: function(models) {
        User.hasMany(UserCard, { foreignKey: 'user_id' })
      }
    }
  })

  const UserCard = sequelize.define('UserCard', {
    id: Sequelize.STRING,
    user_id: Sequelize.STRING,
    card_number: Sequelize.STRING,
    cvv: Sequelize.INTEGER
  },{
    classMethods: {
      associate: function(models) {
        UserCard.belongsTo(User, { foreignKey: 'user_id' })
      }
    }
  })
  ```

