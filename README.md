# CHECK EDI CARGOPORT

หน้าเว็บตรวจเอกสารขาเข้าเรือ: เทียบ **MANIFEST** (สายเรือ, `.xls`/`.xlsx`) กับ **ENTER** (แบบฟอร์ม AMENDMENT ของ Cargoport, `.pdf`) ก่อนทำใบขนสินค้าขาเข้า แล้วดาวน์โหลดรายงาน `EDI.xlsx`

ประมวลผลในเบราว์เซอร์ทั้งหมด (SheetJS + pdf.js + ExcelJS) ไม่มีการอัปโหลดไฟล์ไปที่ใด

## วิธีใช้
1. เปิดหน้าเว็บ (GitHub Pages ของ repo นี้) ลาก `MANIFEST.xls` และ `ENTER.pdf` มาวางพร้อมกัน
2. (ถ้ามี) กรอก **SHED NO. ที่แจ้ง** เช่น `0141`
3. กด **ตรวจสอบ** → ดูตาราง / ค้นหา / แสดงเฉพาะที่ไม่ตรง → **ดาวน์โหลด EDI.xlsx**

## สิ่งที่ตรวจต่อ sub-B/L
CONSIGNEE · CONTAINER · STATUS · TOTAL PKG · PACKAGING · G.W. · MEAS. · MARKS · DESCRIPTION · REEFER TEMP · DG · TRANSIT/TRANSHIPMENT ·
SHED NO. · ประเทศปลายทาง · CARGO MOVEMENT · TAX ID vs NOTIFY · แถว TOTAL เทียบกับยอดที่เอกสารแต่ละฉบับระบุเอง

| สี | ความหมาย |
|---|---|
| ขาว | ไม่ผิด |
| เขียว | ผ่าน (ช่องสรุป) |
| แดง ⚠ | ไม่ตรง / จุดเร่งด่วน |
| ส้ม | ต้องยืนยันด้วยคน |
| เหลือง | B/L อยู่ใน MANIFEST แต่ไม่มี ENTER เทียบ |

- **STATUS**: CY / CY-CY / FCL / ลากตู้ = `CY` · LCL / ขน = `LCL` · CFS / LCL-CFS / เปิดตู้ = `LCL/CFS`
- **MARKS / DESCRIPTION** เทียบเข้ม — ยอมต่างแค่ช่องว่างและเครื่องหมายวรรคตอน
- **DG / REEFER / TRANSIT** พบฝั่งใดก็เตือนเสมอ
- **B/L ที่มีใน ENTER แต่ไม่มีใน MANIFEST** = แถววิกฤต (ของอาจตกหล่นจากใบขน)

## รูปแบบ ENTER ที่รองรับ
แบบฟอร์ม **Cargoport (Thailand) Ltd. "AMENDMENT" (FM-IMP-02)**: ตารางแยกคอลัมน์ตามตำแหน่ง x
(HB/L NO · MARKS & NUMBERS · QUANTITY · DESCRIPTION · CONSIGNEE · G.W./MEAS. · D/O NO.)
เลข B/L ย่อยพิมพ์เป็นเลขฐานบรรทัดหนึ่ง ตามด้วยตัวอักษรต่อท้ายอีกบรรทัด, เลขตู้ (`CONT NO.`) และ `STATUS` พิมพ์ครั้งเดียวต่อทั้งฉบับ
ถ้าอัปโหลดแบบฟอร์มของ forwarder อื่น หน้านี้จะแจ้งว่าอ่านไม่ได้

## โครงไฟล์
- `index.html` — หน้าเว็บ + ส่งออก Excel
- `core.js` — ตรรกะอ่านไฟล์และตรวจ (พอร์ตจาก `build_edi.py` ของสกิล `manifest-enter-edi-check`)

> ไฟล์ MANIFEST/ENTER จริงของลูกค้าห้าม commit (`input/`, `output/` ถูก ignore ไว้แล้ว)
