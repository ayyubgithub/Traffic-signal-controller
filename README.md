# Traffic_Signal_Controller

![Screenshot (2474)](https://github.com/user-attachments/assets/e1430f97-b4e2-438f-91f3-696339f81b9f)

> Given Specification
- The traffic signal for the main highway gets highest priority because cars are continuously present on the main highway. Thus, the main highway signal remains green by default.
- Occasionally, cars from the country road arrive at the traffic signal. The traffic signal for the country road must turn green only long enough to let the cars on the country road go.
- As soon as there are no cars on the country road, the country road traffic signal turns yellow and then red and the traffic signal on the main highway turns green again.
- There is a sensor to detect cars waiting on the country road. The sensor sends a signal X as input to the controller. X = 1 if there are cars on the country road; otherwise, X=0.
- There are delays on transitions from S1 to S2, from S2 to S3, and from S4 to SO. The delays must be controllable.

 > FINITE STATE MACHINE (FSM) DIAGRAM.
  
![Screenshot (2476)](https://github.com/user-attachments/assets/a3a03f14-445f-4ad8-9438-f269a408c011)

## Simulation Result (Waveform)

![Screenshot (2478)](https://github.com/user-attachments/assets/3f18ca54-f59b-4e94-9d10-143c1dc2bd33)

## Schematic of RTL

![Screenshot (2479)](https://github.com/user-attachments/assets/c275ab17-17fb-4f18-a2bf-57a1d87724f8)

