Automated Lawn Mower

An Arduino-controlled lawn mower prototype that drives itself around a yard and steers away from obstacles in real time, using a single ultrasonic sensor mounted on a servo to "look" left and right before deciding which way to turn.

Built as a hardware prototype project — see Lawn Mower Report.pdf for the full writeup and Lawn Mower Images.pdf / Prototype Testing Videos.mp4 for the physical build.

How it works

The mower drives forward continuously while an ultrasonic sensor watches the distance ahead. When it detects an obstacle within range, it stops, backs up briefly, then sweeps a servo-mounted sensor left and right to measure open space on both sides. Whichever side reports more clearance, it pivots toward — then resumes driving forward.

drive forward
  → obstacle within 45 cm?
      → stop → reverse → stop
      → look right, look left (servo sweep + ultrasonic ping)
      → pivot toward the side with more clearance
  → repeat

This is a reactive (not mapped/planned) navigation strategy — there's no memory of the yard, no path planning, and no GPS. It's pure obstacle-avoidance, which keeps the logic small enough to run entirely on an Arduino with no external compute.

Hardware
Component	Role
Arduino (Uno/Nano-class board)	Runs the control loop
Ultrasonic distance sensor (HC-SR04-class, via NewPing)	Obstacle detection, trig on A1, echo on A2
Servo motor	Sweeps the ultrasonic sensor left/right to scan for clearance, signal on pin 13
2x DC drive motors + H-bridge driver	Differential drive (tank-style steering)
Chassis + wheels + battery pack	See Lawn Mower Images.pdf for the physical build

Motor driver pins used in the sketch:

Function	Pin
Left motor forward	7
Left motor backward	6
Right motor forward	5
Right motor backward	4

Note: this sketch only handles navigation/obstacle avoidance. It does not include the actual cutting-blade motor control — add that separately if your build includes one, and make sure it's on its own switch/relay for safety.

Tech stack
Layer	Choice	Why
Controller	Arduino (C++ / .ino)	Simple, real-time, no OS overhead needed for a tight sensor-motor loop
Distance sensing	NewPing library	Cleaner, non-blocking-ish ultrasonic pings than manual pulseIn
Steering	Servo library + differential drive	Lets one sensor scan both directions instead of needing two fixed sensors
Getting started
Install the Arduino IDE.
Install the NewPing and Servo libraries via Library Manager (Servo ships with the IDE by default).
Wire up the motors, H-bridge, ultrasonic sensor, and servo per the pin table above.
Open Lawn_Mower.ino, select your board and port, and upload.
Power the motor driver from the battery pack (not the Arduino's 5V rail) to avoid brownouts when the motors draw current.
Tuning

A few constants in the sketch are worth adjusting for your specific chassis and yard:

Constant	Default	Effect
Obstacle trigger distance	45 cm	Lower = mower gets closer to obstacles before reacting
Turn duration	900 ms	Higher = sharper/longer pivot turns
Max sensor range	200 cm	Ultrasonic sensor's effective ceiling
Roadmap
 Add blade motor control with a dedicated safety cutoff/kill switch
 Perimeter wire or boundary detection so it stays inside the yard
 Replace fixed 45 cm threshold with distance-based speed ramping
 Add a bump sensor as a backup to the ultrasonic sensor for close-range misses
 Battery-level monitoring with auto-return-to-dock

Built as a hardware/robotics portfolio project.
