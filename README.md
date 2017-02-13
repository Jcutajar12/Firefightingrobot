# Firefightingrobot
/*
Detect if a fire is in the robots path, and if so turn around and put the fire out with a connect fan.
*/
#include "simpletools.h"
#include "abdrive.h"
#include "ping.h"
#include "fdserial.h"
#include "servo.h"

int pings = 0;
int nofire = 0;
int beep = 0;
int goto1 = 2;
int goto2 = 2;
int goto3 = 2;
int fan = 0;
int P16;
int main()
{
drive_speed(32,32);
drive_setRampStep(10);

while ((pings < 1000) && (nofire == 0))
{
pause(3);

if(ping_cm(8) < 10)
{
pings = 4000;
nofire = 1;
beep = 1;
goto1 = 1;
goto2 = 0;
goto3 = 0;
fan = 1;
}

pings = pings + 1;

}

if(goto1 == 1)
{
drive_speed(0,0);
drive_goto(26,-25);
drive_goto(25,25);
drive_goto(26,-25);
drive_speed(32,32);
drive_speed(0,0);
goto1 = 0;
pings = 0;
}

if(goto1 == 2)
{
drive_speed(0,0);
drive_goto(26,-25);
drive_goto(25,25);
drive_goto(26,-25);
drive_speed(32,32);
pings = 0;
}

while ((pings < 1000) && (nofire == 0))
{
pause(3);
if(ping_cm(8) < 10)
{

pings = 4000;
nofire = 1;
beep = 1;
goto3 = 0;
goto1 = 0;
goto2 = 1;
fan = 1;
}

pings = pings + 1;

}
if(goto2 == 1)
{
drive_speed(0,0);
drive_goto(-25,26);
drive_goto(25,25);
drive_goto(-25,26);
drive_speed(32,32);
drive_speed(0,0);
pings = 0;
goto2 = 0;
}

if(goto2 == 2)
{
drive_speed(0,0);
drive_goto(-25,26);
drive_goto(25,25);
drive_goto(-25,26);
drive_speed(32,32);
pings = 0;
}

while ((pings < 1000) && (nofire == 0))
{
pause(3);
if(ping_cm(8) < 10)
{

pings = 4000;
nofire = 1;
beep = 1;
goto1 = 0;
goto2 = 0;
goto3 = 1;
fan = 1;
}

pings = pings + 1;
}

if(goto3 == 1)
{
drive_speed(0,0);
drive_goto(-25,26);
drive_goto(25,25);
drive_goto(-25,26);
drive_speed(0,0);
pings = 0;
goto3 = 0;
}

if(goto3 == 2)
{
drive_speed(0,0);
drive_goto(-25,26);
drive_goto(25,25);
drive_goto(-25,26);

pings = 0;
}

while(fan == 1)
{
servo_speed(P16,64);
}

}
