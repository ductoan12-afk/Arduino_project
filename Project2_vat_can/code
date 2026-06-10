#include <DHT.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define DHTPIN 2       
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

// Địa chỉ màn hình LCD đã test chạy ngon ở mạch của ông
LiquidCrystal_I2C lcd(0x27, 16, 2);

const int ledCanhBao = 8;     

void setup() {
  dht.begin();
  lcd.init();
  lcd.backlight();
  
  lcd.setCursor(0, 0);
  lcd.print("He thong IoT");
  lcd.setCursor(0, 1);
  lcd.print("Dang ket noi...");
  delay(2000);
  lcd.clear();
  
  pinMode(ledCanhBao, OUTPUT);
}

void loop() {
  float doAm = dht.readHumidity();
  float nhietDo = dht.readTemperature();

  if (isnan(doAm) || isnan(nhietDo)) {
    lcd.setCursor(0, 0);
    lcd.print("Loi cam bien!   ");
    lcd.setCursor(0, 1);
    lcd.print("Kiem tra lai day");
    return;
  }

  // CỜ BÁO ĐỘNG (Dùng để kiểm tra xem có bất kỳ lỗi nào không để bật LED)
  bool coSuCo = false;

  // --- KHỐI XỬ LÝ 1: KIỂM TRA NHIỆT ĐỘ ---
  lcd.setCursor(0, 0);
  if (nhietDo > 40.0) {
    lcd.print("T:Qua Cao! NGUYHIEM"); 
    coSuCo = true;
  } 
  else if (nhietDo < 5.0) {
    lcd.print("T:Qua Thap!NGUYHIEM");
    coSuCo = true;
  } 
  else {
    lcd.print("Nhiet do: ");
    lcd.print(nhietDo, 1);
    lcd.print(" C  ");
  }

  // --- KHỐI XỬ LÝ 2: KIỂM TRA ĐỘ ẨM ---
  lcd.setCursor(0, 1);
  if (doAm > 89) {
    lcd.print("H: Do am qua cao   ");
    coSuCo = true;
  } 
  else if (doAm < 20.0) {
    lcd.print("H: Do am qua thap  ");
    coSuCo = true;
  } 
  else {
    lcd.print("Do am:    ");
    lcd.print(doAm, 1);
    lcd.print(" %  ");
  }

  // --- KHỐI ĐIỀU KHIỂN ĐÈN LED CẢNH BÁO ---
  if (coSuCo == true) {
    digitalWrite(ledCanhBao, HIGH); // Bật đèn khi rơi vào bất kỳ trường hợp bất thường nào
  } else {
    digitalWrite(ledCanhBao, LOW);  // Tắt đèn khi mọi thông số an toàn
  }

  delay(2000); // Đợi 2 giây rồi đọc lại dữ liệu
}
