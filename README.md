# Voice_controlled_robot
/*Voice Controlled Robot using Speed Sensor  Developed a prototype robot is be controlled by the given voice commands from the phone*/
include<SoftwareSerial.h>
String voice,sid;
 int s;
const byte MOTOR_A = 3;  // Motor 2 Interrupt Pin - INT 1 - Right Motor
 // Constant for steps in disk
const float stepcount = 20.00;  // 20 Slots in disk, change if different
 // Integers for pulse counters
volatile int counter = 0;
// Motor A
 int in1 = 7;
int in2 = 8;
 
// Motor B
 int in3 = 9;
int in4 = 10;
// Interrupt Service Routines
 // Motor A pulse count ISR
void ISR_count()  
{
  counter++;  // increment Motor A counter value
} 
 
// Function to convert from centimeters to steps
int CMtoSteps(int cm) {
 
  int result;  // Final calculation result
  float circumference = (68.10 * 3.14) / 10; // Calculate wheel circumference in cm
  float cm_step = circumference / stepcount;  // CM per Step
  
  float f_result = cm / cm_step;  // Calculate result as a float
  result = (int) f_result; // Convert to an integer 
  
  return result;  // End and return result
 
}
 
// Function to Move Forward
void MoveForward(int steps)//, int mspeed) 
{
   counter= 0;  //  reset counter A to zero
   // Set Motor A forward
   digitalWrite(in1, HIGH);
   digitalWrite(in2, LOW);
 
   // Set Motor B forward
   digitalWrite(in3, HIGH);
   digitalWrite(in4, LOW);
   
   // Go forward until step value is reached
   while (steps > counter ) {
   
    if (steps > counter) {
   digitalWrite(in1, HIGH);
   digitalWrite(in2, LOW);
 
   // Set Motor B forward
   digitalWrite(in3, HIGH);
   digitalWrite(in4, LOW);
    }
    else 
    {
   digitalWrite(in1, HIGH);
   digitalWrite(in2,HIGH );
 
   // Set Motor B forward
   digitalWrite(in3, HIGH);
   digitalWrite(in4, HIGH);
    }
}
    
  // Stop when done
  digitalWrite(in1, HIGH);
   digitalWrite(in2,HIGH );
 
   // Set Motor B forward
   digitalWrite(in3, HIGH);
   digitalWrite(in4, HIGH);
  counter= 0;  //  reset counter to zero
 
}
 
// Function to Move in Reverse
void MoveReverse(int steps)//, int mspeed) 
{
   counter = 0;  //  reset counter  to zero
   
   // Set Motor A reverse
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
 
  // Set Motor B reverse
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
   
   // Go in reverse until step value is reached
   while (steps > counter) {
   
    if (steps > counter) {
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);
    } 
    else
    {
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, LOW);
    }
   }
    
//  // Stop when done
   digitalWrite(in1, LOW);
   digitalWrite(in2, LOW);
 
  // Set Motor B reverse
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
  counter= 0;  //  reset counter to zero
 
}
 
// Function to Spin Right
void Right(int steps)//, int mspeed) 
{
   counter = 0;  //  reset counter  to zero
   // Set Motor A stop
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
 
  // Set Motor B forward
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
   
   // Go until step value is reached
   while (steps > counter) {
   
    if (steps > counter) {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
    } 
    else {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
    }
 }
    
// Stop when done
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);

  counter = 0;  //  reset counter A to zero
}
 
// Function to Left
void Left(int steps)//, int mspeed) 
{
   counter = 0;  //  reset counter A to zero
   // Set Motor A forward
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
 
  // Set Motor B stop
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
   
   // Go until step value is reached
   while (steps > counter) {
   if (steps > counter) {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
    }
    else {
   digitalWrite(in1, LOW);
   digitalWrite(in2, LOW);
   digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
    }
}
    
  // Stop when done
 digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
 digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
  counter = 0;  //  reset counter to zero
 
}
 
void setup() 
{
  // Attach the Interrupts to their ISR's
  pinMode(MOTOR_A,INPUT);
  attachInterrupt(digitalPinToInterrupt (MOTOR_A), ISR_count, RISING);  // Increase counter A when speed sensor pin goes High
  pinMode(in1,OUTPUT);
  pinMode(in2,OUTPUT);
  pinMode(in3,OUTPUT);
  pinMode(in4,OUTPUT);

Serial.begin(9600);
} 
 
 
void loop()
{
  while(Serial.available()>0)
 {
 voice=Serial.readString();
 Serial.println(voice);
 char x=' ';
 int b=voice.indexOf(x);
 String dis=voice.substring(0,b);
 String sid=voice.substring(b+1);
 Serial.println(sid);
 int s=dis.toInt(); 
if(voice.length()){
if(sid=="forward")
  {
    MoveForward(CMtoSteps(s));
  }
  else if(sid=="reverse")
  {
    MoveReverse(CMtoSteps(s));
  }
   else if(sid=="left")
  {
    Left(CMtoSteps(s));
  }
  else if(sid=="right")
  {
    Right(CMtoSteps(s));
  }
}
}

}
