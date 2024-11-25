// Set the LCD address to 0x27 for a 16 chars and 2 line display
LiquidCrystal_I2C lcd(0x3F, 16, 2);

int RedLED = 10;            // Red LED Pin connected to PIN 10
int GreenLED = 9;           // Green LED Pin connected to PIN 9
int BlueLED = 11;           // Blue LED Pin connected to PIN 11
int Buzzer = 3;             // Buzzer Pin connected to PIN 3
int MotionSensor = 2;       // Sensor Pin connected to PIN 2
int FlameSensor = A0;       // Flame Pin connected to PIN A0;

int MotionDetected = LOW;   // Start MotionDetected as LOW (No motion detected)
int FlameVal = LOW;         // Start FlameVal as LOW

int S1=4; 
int S2=5; 
int S3=6;

int stage; 
int timeDelay=0; 
int timerCount=0;

int DIP1; 
int DIP2; 
int DIP3;

void setup() {
    Serial.begin(9600);      // Set serial out if we want debugging
    pinMode(GreenLED, OUTPUT);  // declare Green LED as output
    pinMode(RedLED, OUTPUT);    // declare Red LED as output
    pinMode(BlueLED, OUTPUT);   // declare Blue LED as output
    pinMode(MotionSensor, INPUT);  // declare the PIR sensor as input
    pinMode(Buzzer, OUTPUT);    // declare buzzer as output
    pinMode(FlameSensor, INPUT);   // declare flame as input

    pinMode(S1, INPUT); 
    pinMode(S2, INPUT); 
    pinMode(S3, INPUT); 

    DIP1 = digitalRead(S1); 
    DIP2 = digitalRead(S2); 
    DIP3 = digitalRead(S3);


if(DIP1==HIGH && DIP2==LOW && DIP3==LOW)
{
    timerDelay=10;
}
else if(DIP1==HIGH && DIP2==HIGH && DIP3==LOW)
{
    timerDelay=20;
}
else if(DIP1==HIGH && DIP2==HIGH && DIP3==HIGH)
{
    timerDelay=30;
}

timerCount=0;
Serial.println(" ");
Serial.print("Max Timer Delay (sec): ");
Serial.println(timerDelay);
lcd.backlight();
lcd.begin();
lcd.setCursor(0,0);
lcd.print("System ON ");

delay(2000); // Allow time for the PIR Sensor to calibrate

void loop() {
    // put your main code here, to run repeatedly:
    MotionDetected = digitalRead(MotionSensor);  // Read the PIR sensor
    FlameVal = analogRead(FlameSensor);          // Read the Flame Sensor

    Serial.print("Fire: ");
    Serial.print(FlameVal);

    Serial.print(",   Motion: ");
    Serial.print(MotionDetected);
    Serial.println(" ");

    // Decision Making
    if(MotionDetected == HIGH && stage==2 || FlameVal > 50){
        stage=1; // idle stage
    }


    if(FlameVal < 30 && stage==1){
        stage=2; // Fire on, motion off
    }

    if(MotionDetected == HIGH && stage==2)
    {
        stage=3; // restart timer
    }

    if(MotionDetected == LOW && stage==2)
    {
        stage=5; // Timer set
    }


// Execute action

if(stage==1) // All off, idle
{
    lcd.setCursor(0,1);
    lcd.print("IDLE        ");
    Serial.println("IDLE        ");
    digitalWrite(Buzzer, LOW);
    digitalWrite(RedLED, LOW);
    digitalWrite(GreenLED, LOW);
    digitalWrite(BlueLED, LOW);
    Serial.print("IDLE");
}
else if(stage==2) // Fire on, motion off
{
    Serial.println(FlameVal);
    lcd.setCursor(0,1);
    lcd.print("Fire Detected    ");
    Serial.println("Fire Detected    ");
    digitalWrite(RedLED, HIGH);
    digitalWrite(GreenLED, LOW);
    digitalWrite(BlueLED, LOW);
    digitalWrite(Buzzer, HIGH);
    delay(500);
}
else if(stage==3) // Fire on and Motion on
{
    lcd.backlight();
    lcd.setCursor(0,1);
    lcd.print("F=1.M=1.Tmr OFF ");
    Serial.println("F=1.M=1.Tmr OFF ");
    digitalWrite(RedLED, LOW);
    digitalWrite(GreenLED, HIGH);
    digitalWrite(BlueLED, LOW);
    digitalWrite(Buzzer, LOW);
    delay(500);
    stage=1;
}

else if(stage==4) //Trigger Buzzer
{
    lcd.backlight();
    lcd.setCursor(0,1);
    lcd.print("TMR Alarm ON ");
    Serial.print("TMR Alarm ON ");
    digitalWrite(RedLED, HIGH);
    digitalWrite(GreenLED, LOW);
    digitalWrite(BlueLED, LOW);
    digitalWrite(Buzzer, HIGH);
    delay(500);
}

else if(stage==5) //Timer set
{
    timerCount++;
    if(timerCount<=timerDelay)
    {
    lcd.setCursor(0,1);
    lcd.print("Timer: ");
    lcd.print(timerCount);
    Serial.print("Increment Counter");
    }else{
    lcd.setCursor(0,1);
    lcd.print("Timer UP!! ");
    lcd.print(timerCount);
    Serial.print("Timer End ");
    stage=4;    //Trigger Alarm
}
    delay(1000);

}else{

    Serial.println("Err. No decision");

}
}




