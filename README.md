# CEG3006-Project-Group-8


# System Integration

## Overall Solution

The proposed Vehicle-to-Pedestrian (V2P) solution aims to improve child pedestrian safety in scenarios where behaviour can be highly unpredictable, such as sudden running or unexpected entry onto the road. In this concept, the child is equipped with a wearable device, such as a smartwatch with the proposed application installed. The application uses the device’s built-in sensors, including motion and location-related data, to monitor the child’s movement and determine whether the child is within a predefined blind-spot or danger zone.

When the system detects movement patterns that suggest the child may be approaching or entering the road in a potentially dangerous manner, an alert is transmitted to nearby vehicles through BLE-based V2P communication. To further strengthen safety in blind-spot situations, a second layer of protection is introduced: the first vehicle that receives the alert can rapidly forward the warning to other approaching vehicles through low-latency V2V communication. This allows vehicles in the surrounding vicinity to receive an early warning even if the child is not yet directly visible, thereby giving drivers more time to slow down or stop and reducing the risk of a collision.


## Literature Review

Vehicle-to-Pedestrian (V2P) safety systems have been proposed as a way to reduce collisions involving vulnerable road users by allowing pedestrians and vehicles to exchange real-time information such as location, speed, and direction. A recent study by Certad et al (2025), found that current V2P systems still face an important limitation: they often do not adequately account for the uncertainty and unpredictability of pedestrian behaviour, which can reduce the accuracy of collision-risk prediction. The paper also showed that V2P warnings can be more effective than conventional auditory warnings, particularly when pedestrians are distracted by smartphone use or headphones. 

Sewalkar et al (2019), also noted that V2P communication identifies children as a particularly challenging pedestrian group due to their tendency to move suddenly, change direction unpredictably, and exhibit less consistent road behaviour. This further notes that although smartphones are widely adopted in V2P systems because of their embedded sensors and communication functions, they may not always be the most appropriate standalone device for child pedestrians, thereby creating potential for supplementary wearable devices such as tags or smartwatches. Furthermore, while smartphone-based V2P communication offers practical implementation advantages, its effectiveness may be influenced by factors such as signal obstruction, link quality, and communication delay. 

Similarly, Gelbal et al (2024), proposed a smartphone-based V2P safety system that uses mobile-phone sensors to predict pedestrian motion and communicate warnings via Bluetooth and internet-based connectivity. Their work demonstrates the feasibility of using phone sensor data for pedestrian motion prediction and driver warning, but it focuses mainly on general pedestrian safety rather than a child-specific use case.

Hence, based on these gaps, the proposed solution focuses specifically on child pedestrians by combining a wearable watch and application. The application can provide important child-specific context such as age, while the wearable contributes motion data to detect sudden running behaviour for the system processing which will then send out a BLE packet to the nearest vehicle. Unlike existing solutions, the system also introduces a second warning layer, where the nearest vehicle will further relay the alert to other approaching vehicles through low-latency V2V communication to extend warning coverage to nearby vehicles that do not yet have direct line-of-sight to the child or blind spot scenarios.



## System Architecture

<p align='center'>
<img alt="image" src="https://github.com/user-attachments/assets/151f1925-3976-4ca6-bad5-b757e70b650b" width="500"/>
</p>
The proposed system architecture consists of three main subsystems: the pedestrian subsystem, the communication subsystem, and the vehicle subsystem. On the pedestrian side, a smartwatch worn by the child continuously monitors location and motion data using onboard sensors such as GPS, an accelerometer and a gyroscope. The smartwatch application checks whether the child is within a predefined geofenced blindspot or hazardous roadside zone and evaluates whether the child’s movement pattern indicates an approach toward the road.

When the trigger conditions are met, the smartwatch generates a BLE-based warning message and broadcasts it to vehicles within range of 15metres. On the vehicle side, an On-Board Unit (OBU) or compatible head unit receives the message, validates its freshness and uniqueness, and displays a warning to the driver. To extend situational awareness, the first receiving vehicle may rebroadcast the warning to nearby vehicles within a range of 50metres. This forwarding process is controlled with a hop count of 2 hops and message expiry time of 1 minute to prevent unnecessary network congestion.

In addition, the system uses predefined geofenced blindspot zones, which may be configured using stored coordinates representing hazardous crossing areas. These zones allow the smartwatch to apply context-aware alert triggering rather than relying solely on motion data.

## Functions

### Smartwatch Functions

| No. | Function | Description |
|---|---|---|
| 1 | **Location Tracking Function** | Continuously obtains the child’s current location using the smartwatch GPS or paired location service. |
| 2 | **Geofence Detection Function** | Checks whether the smartwatch is within a predefined blindspot or hazardous roadside zone. |
| 3 | **Motion Monitoring Function** | Collects motion data from the accelerometer and gyroscopee to observe the child’s movement pattern. |
| 4 | **Motion Analysis Function** | Processes motion data readings to determine whether the child is moving at a speed or pattern consistent with approaching the road. |
| 5 | **Risk Evaluation Function** | Combines geofence status and motion analysis to decide whether the situation is risky enough to trigger an alert. |
| 6 | **Alert Trigger Function** | Activates the warning process when the trigger conditions are met, such as entering a danger zone while moving quickly toward the road. |
| 7 | **BLE Beacon Broadcast Function** | Broadcasts a BLE alert packet to nearby vehicles once a risk event is detected. |
| 8 | **Alert Repeat / Retransmission Function** | Repeats the BLE warning for a short predefined period to increase the chance that nearby vehicles receive it. |
| 9 | **Event ID Generation Function** | Assigns a unique identifier to each alert event so duplicate warnings can be recognised. |
| 10 | **Alert Expiry Function** | Stops broadcasting once the event duration ends or the child is no longer in the risk condition. |

### Vehicle-side Functions

| No. | Function | Description |
|---|---|---|
| 1 | **BLE Scanning Function** | Continuously scans for incoming BLE warning signals from nearby smartwatches or forwarded vehicles. |
| 2 | **Alert Reception Function** | Receives and extracts the warning message fields from the BLE packet. |
| 3 | **Message Validation Function** | Checks whether the received alert is valid, fresh, and not expired. |
| 4 | **Duplicate Detection Function** | Determines whether the same alert has already been received before, using the message ID. |
| 5 | **Driver Warning Function** | Displays a visual and/or audio warning on the dashboard or head unit to inform the driver of a child approaching or crossing ahead. |
| 6 | **Risk Prioritisation Function** | Determines the urgency of the warning based on factors such as signal freshness, danger zone, and message type. |
| 7 | **Vehicle Rebroadcast Function** | Forwards the alert to other nearby vehicles within a limited range. |
| 8 | **Hop Count Control Function** | Tracks how many times the message has been forwarded and prevents excessive rebroadcasting. |
| 9 | **Message Expiry Handling Function** | Removes old alerts from the system once their validity period has passed. |

### System / Network Functions

| No. | Function | Description |
|---|---|---|
| 1 | **Geofence Management Function** | Stores and manages the coordinates and boundaries of blindspot or hazardous crossing zones. |
| 2 | **Message Formatting Function** | Packages alert information into a standard BLE message structure for transmission. |
| 3 | **Timestamping Function** | Adds the event creation time to each message so vehicles can judge whether it is still relevant. |
| 4 | **TTL (Time-to-Live) Function** | Sets a lifetime for each message to prevent stale warnings from remaining in the network. |
| 5 | **Security / Identity Protection Function** | Ensures the transmitted message does not reveal the personal identity of the child, only the required safety alert information. |
| 6 | **Logging Function** | Records triggered events, received alerts, and rebroadcast actions for later analysis or testing. |

## System Flowchart
<p align='center'>
<img alt="image" src="https://github.com/user-attachments/assets/efd2793d-4e88-44e4-b0aa-4d8575fd40ee" width="380" />
</p>

The system begins by continuously monitoring the child’s location and motion data through the smartwatch. When the smartwatch detects that the child has entered a predefined geofenced blindspot or hazardous roadside area, the accelerometer and gyroscope data is further analysed to determine whether the child is moving toward the road at a potentially dangerous speed or pattern. If both conditions are satisfied, the smartwatch generates and broadcasts a BLE-based warning message to vehicles within a 15metre range of .

A nearby vehicle equipped with a compatible receiver then receives the alert, validates the message, and warns the driver through the vehicle dashboard or head unit. To improve awareness for approaching vehicles, the first receiving vehicle may rebroadcast the message to nearby vehicles within a range of 50metres. The forwarding process is controlled using message expiry time and hop count to prevent excessive rebroadcasting.


## Hardware Components and Parameters

| Component | Role in System | Communication / Sensor Type | Key Parameters | Notes |
|---|---|---|---|---|
| Smartwatch | Worn by child to monitor movement and trigger alerts | Accelerometer, gyroscope, GPS, BLE | Motion sensing: continuous; BLE alert range: ~15 m; GPS accuracy: location-dependent; Trigger latency: low | Runs the proposed child safety application and performs geofence + motion-based risk detection |
| Accelerometer | Detects sudden movement or acceleration changes | Motion sensor | Sampling rate: configurable; Low data rate; Very low latency | Used to detect sudden running or movement toward the road |
| Gyroscope | Monitors change in orientation and motion behaviour | Motion sensor | Sampling rate: configurable; Low data rate; Very low latency | Supports motion pattern analysis together with the accelerometer |
| GPS / Location Service | Determines whether child is within predefined blind-spot / danger zone | Location sensing | Range: global; Accuracy: environment-dependent; Update latency: moderate | Used for geofencing, but may be less accurate in dense urban areas |
| BLE Module (Smartwatch) | Broadcasts child-risk alert to nearby vehicles | Bluetooth Low Energy | Range: ~15 m; Low bandwidth; Low power; Low latency | Used for short-range V2P alert transmission from the smartwatch |
| Vehicle OBU / Head Unit | Receives smartwatch alert and warns the driver | BLE receiver + in-vehicle interface | Alert reception latency: low; Short-range reception | Processes alert packet and displays warning on dashboard/head unit |
| Vehicle Dashboard / HMI | Displays warning to the driver | Visual / audio interface | Alert display latency: near real-time | Can show messages such as "Child approaching road ahead" |
| Vehicle V2V Communication Module | Rebroadcasts warning to nearby vehicles | V2V wireless communication | Rebroadcast range: ~50 m; Low-latency communication; Bandwidth depends on V2V technology | Extends the warning beyond the initial smartwatch BLE range |
| Smartphone / Admin Device | Used only for setup, profile creation, or app configuration | App configuration interface | Not required during live operation | Can be used to register child profile and set danger zones if needed |


| Parameter | Proposed Value / Target | Description |
|---|---|---|
| Smartwatch-to-vehicle alert range | ~15 m | BLE transmission range for the initial child-risk alert |
| Vehicle-to-vehicle rebroadcast range | ~50 m | Range for forwarded warning to nearby vehicles |
| Communication latency | Low-latency / near real-time | Important so vehicles can react early in blind-spot scenarios |
| Bandwidth requirement | Low | Only short warning packets are transmitted, so high bandwidth is not required |
| Power consumption | Low | BLE consumes less energy making it viable for smartwatches |
| Motion sensing response | Real-time | Sensors monitor for sudden child movement (running) |
| Geofence trigger area | Predefined blind-spot / danger zone | Alert logic only activates in hazardous roadside areas |
| Message lifetime (TTL) | 500ms | Prevents stale alerts from being rebroadcast indefinitely |
| Hop count | Limited | Controls how many times the warning can be forwarded |
| Driver alert type | Visual and/or audio | Warning shown on dashboard or head unit |


## Real-life Scenario Use Case
<p align='center'>
<img alt="image" src="https://github.com/user-attachments/assets/831f7e57-8180-43aa-98f0-a167e9df72a5" />
</p>

In a typical roadside environment, a child may be walking or playing near a road where visibility is obstructed by fences, trees or walls. These roadside obstacles create blind spots, making it difficult for approaching drivers to detect the child early. In such situations, the child may suddenly move toward the roadside or step onto the road from a hidden area, leaving drivers with very limited reaction time.

To address this, the proposed V2P concept uses a dedicated application installed directly on the child’s smartwatch. A user profile is first created within the application to identify the wearer as a child, allowing the system to operate according to the intended safety use case. The application utilizes the smartwatch’s built-in sensors, including the accelerometer, gyroscope, and GPS location services, to continuously monitor both the child’s movement and location.
When the smartwatch detects that the child has entered a predefined blind spot or danger zone through geofencing, the system begins evaluating the child’s motion data more closely. By analysing changes in acceleration, movement behaviour, and motion patterns, the application determines whether the child is likely approaching the road in a potentially dangerous manner. Once a risk is detected, the smartwatch immediately broadcasts a BLE alert packet to the nearest vehicle within a 15-metre range.

A nearby vehicle that receives the alert will display a warning on the vehicle dashboard or head unit to notify the driver that a child may be approaching or entering the roadway from a blind-spot area. To further improve safety beyond the initial detection range, the receiving vehicle then rebroadcasts the alert to other vehicles within a 50-metre range via DSRC packet. This allows drivers in the surrounding vicinity to be warned in advance, even before they reach the blind-spot area or gain direct visual contact with the child.

As a result, the system provides an early-warning mechanism that improves driver awareness in blind-spot environments and gives both nearby and approaching vehicles more time to slow down or stop. This helps reduce the likelihood of collisions involving children in roadside danger zones.



# Decision Logs

This document records the team’s key design decisions from **Decision Log #1 to Decision Log #18**, including the trigger or problem, options considered, evaluation criteria, and final rationale behind each selected direction.

---

<details>
<summary><strong>Decision Log #1 — Selecting the Initial Target Problem and User Group</strong></summary>

### Date
08/03/2026

### Trigger / Problem
Based on our research, traffic accidents that caused injuries climbed by about 7% to 7,560 cases in 2025. Deaths from traffic accidents also rose from 139 cases in 2024 to 147 cases in 2025, showing a steady rise since 2022. In addition, red-light-running-related accidents rose by 27.1% from 2024 to 2025 (Sng, 2026). Based on this, the team evaluated and selected the targeted problem and user group.

### Options / Alternatives
- Busy Junction
- Special Pedestrian Groups
- Zebra Crossing

### Evaluation Criteria
- Relevance to the stated objective
- Relevance to a real-world safety issue
- Originality
- Feasibility without first developing a hardware prototype
- Ability to justify with existing literature and solutions
- Clarity of target user group focus

### Decision and Rationale
The team decided to focus on **special groups of pedestrians**, particularly the elderly.

Elderly pedestrians are at higher risk because they generally have slower walking speed, reduced agility, longer response time, and lower situational awareness compared to younger adults. This becomes even more serious as red-light-running-related accidents continue to increase, despite pedestrians often having a false sense of security when crossing because they have the right of way.

When drivers attempt to beat the red light, pedestrians may need to react quickly to avoid danger. Therefore, there is a need to notify both pedestrians and drivers earlier so that both parties can avoid accidents from happening.

### AI Usage
AI was used to brainstorm possible V2P application areas and help compare them. The final choice and rationale were selected by the team.

### Team Members
- Wen Hui
- Khee Xuan
- Yuankai
- Daniel
- Zelda
- Syaakir

</details>

---

<details>
<summary><strong>Decision Log #2 — Selecting the Initial Solution Direction</strong></summary>

### Date
03/03/2026

### Trigger / Problem
Based on the selected issue identified in Decision Log #1, namely elderly pedestrians’ safety at traffic junctions, with a focus on jaywalking and unsafe crossing behaviour at signalised intersections, this decision log evaluates and selects a suitable solution to address the problem.

### Options / Alternatives
- Roadside unit / smart traffic light based warning system
- Smartwatch-based alerting system to pedestrian
- Installation of camera to identify pedestrian
- Different sound cue for traffic light when abnormal road behaviour is detected

### Evaluation Criteria
- Relevance to the stated objective
- Relevance to a real-world safety issue
- Originality
- Feasibility without first developing a hardware prototype
- Ability to justify with existing literature and solutions
- Clarity of target user group focus

### Decision and Rationale
The team selected a **roadside-based pedestrian monitoring and warning system**.

Using cameras or infrared sensors installed onto existing traffic lights together with computer vision, the system detects whether pedestrians are still within the crossing area after the traffic signal changes. When this occurs, a warning is sent through the vehicular network to the vehicle’s OBU, which alerts the driver of pedestrians ahead and advises them to slow down or wait before proceeding.

This option was chosen because it directly addresses the safety risk at signalised intersections and does not depend on pedestrians carrying smartphones or wearable devices. It provides real-time detection of actual crossing conditions, supports inclusive use for elderly or slower-moving pedestrians, and improves driver awareness before a conflict occurs. As a result, it is expected to reduce potential vehicle-pedestrian collisions and improve safety at busy intersections.

### AI Usage
AI was used to brainstorm possible V2P application areas and help compare them. The final choice and rationale were selected by the team.

### Team Members
- Khee Xuan
- Yuankai

</details>

---

<details>
<summary><strong>Decision Log #3 — Revising the Concept After Feedback</strong></summary>

### Date
08/03/2026

### Trigger / Problem
After consulting with the professor, the team received feedback that the initial concept developed in Decision Log #1 and #2 was more V2I-focused than V2P-focused. In addition, the proposed use of installed cameras to detect pedestrians and provide data to vehicles would increase both system complexity and implementation cost.

The proposed target user group was also not clearly defined, making the concept too broad and vague. Furthermore, the original scenario of jaywalking at a traffic junction was considered unrealistic, since jaywalking is more likely to occur at uncontrolled road sections or between parked vehicles rather than at signalised crossings.

As a result, the team decided to reject the previous approach and revise the concept by identifying a more suitable V2P application, a more appropriate real-world scenario, and a more specific problem statement that was both relevant and feasible.

### Options / Alternatives
- Cyclist warning system
- Wheelchair-user crossing assistance
- Child pedestrian safety
- Driver collision warning for pedestrians
- Early warning for pedestrian hotspot areas
- Jaywalking at uncontrolled junctions
- Junction turn with obstructed turns

### Evaluation Criteria
- Potential real-world safety relevance
- Originality and feasibility of the concept
- Ability to justify the problem using existing literature and current solutions
- Clarity of the target user group within a specific scenario

### Decision and Rationale
The team decided to focus on **child pedestrian safety**.

This option was selected because it provides a clearer and more specific target user group within the V2P context compared to more general pedestrian-focused solutions. Children are more susceptible to road traffic injuries compared to adults due to their immature physical, cognitive, and social development (Oh & Kim, 2025).

Most existing solutions focus on pedestrians as a general group and place minimal focus on child safety, which makes this a meaningful and relevant area for further research, with good potential to be refined into a more specific real-world scenario in later design stages. Compared to the other options, this direction offers a more focused foundation for developing an original concept while remaining feasible within the project scope.

### AI Usage
NA

### Team Members
- Daniel
- Khee Xuan
- Yuankai

</details>

---

<details>
<summary><strong>Decision Log #4 — Refining the Problem Area Within Child Pedestrian Safety</strong></summary>

### Date
13/03/2026

### Trigger / Problem
The focus on child pedestrian safety was still too broad. The team therefore needed to further refine the problem area and identify a more specific issue to address within this context. This was important because a clearer and more detailed problem statement would help guide the development of a more targeted and meaningful V2P solution for child pedestrians.

### Options / Alternatives
- Child runs across the road after a ball
- Child pedestrian unpredictable behaviours
- Unable to spot children due to height
- Children unaware of their surroundings

### Evaluation Criteria
- Feasibility of the option
- Ability to justify it using existing literature and current solutions
- Realism of the proposed application
- Relevance of real-world limitations or research gaps

### Decision and Rationale
The team decided to focus on **child pedestrians’ unpredictable behaviours**.

This option was selected because unpredictability in pedestrian movement is recognised as a key challenge in the V2P context (Gelbal et al., 2024). Additionally, accidents involving children are often linked to such unpredictability and their inability to make adequate decisions (Oh & Kim, 2025). Sudden changes in movement can reduce the effectiveness of collision prediction and warning systems and catch drivers off guard.

Within this broader issue, child pedestrians were considered especially relevant because their behaviour near roads is less consistent and more unpredictable (Sewalkar & Seitz, 2019). Therefore, it is more difficult for drivers and vehicle systems to anticipate their movement quickly. Even though predicting a child’s behaviour may not be easy, reducing the likelihood of accidents remains essential. As a result, this problem was seen as a meaningful and researchable direction that could support the development of a more focused V2P safety concept.

### AI Usage
NA

### Team Members
- Daniel
- Syaakir
- Wen Hui

</details>

---

<details>
<summary><strong>Decision Log #5 — Selecting the Specific Real-World Scenario</strong></summary>

### Date
14/03/2026

### Trigger / Problem
Following the earlier decision to focus on child pedestrians’ unpredictable behaviours, the next step was to identify which specific real-world scenario would be the most relevant and suitable for further development. Although children may behave unpredictably in many road situations, selecting a more prominent and realistic scenario was necessary to guide the technical design of the proposed solution and ensure that the concept remained feasible and meaningful.

### Options / Alternatives
- Child pedestrian running or jumping into the road at a junction during a red light
- Child pedestrian running into the road from blind-spot scenarios
- Child pedestrian running at a zebra crossing without checking corners
- Child runs across the road after a ball

### Evaluation Criteria
- Feasibility of the option
- Relevance of the specific scenario
- Likelihood of occurrence
- Presence of real-world accident evidence supporting the scenario

### Decision and Rationale
The team decided to focus on **child pedestrians running onto the road from blind-spot scenarios**.

While all the options were considered valid and relevant, the blind-spot scenario was selected because it presents a higher immediate safety risk to both drivers and child pedestrians.

In junction and zebra crossing situations, drivers are generally more likely to expect pedestrians and may have some visual awareness of nearby children. In contrast, in blind-spot situations, an approaching driver may have little or no visibility of a child who suddenly emerges from behind an obstruction such as a parked vehicle, roadside object, or bush. This can result in serious injuries when the car is travelling at full speed on a clear road.

This significantly reduces the available reaction time and increases the likelihood of a collision. The team also noted that examples of such accidents can be found in real-world incident footage. Overall, it was collectively agreed that blind-spot scenarios represent a more critical and realistic situation for the concept to address moving forward.

### AI Usage
NA

### Team Members
- Daniel
- Zelda
- Syaakir

</details>

---

<details>
<summary><strong>Decision Log #6 — Selecting the Detection / Alert Mechanism</strong></summary>

### Date
15/03/2026

### Trigger / Problem
Based on Decision Log #5, the team had decided to focus on child pedestrians’ unpredictable behaviour, particularly in blind-spot situations. The next step was to evaluate what type of detection or alert mechanism would be most suitable for reducing collision risk in such scenarios.

Since the main concern was not just the presence of a child near the road, but also the possibility of sudden risky movement, the team needed to determine which approach could better capture this behaviour and provide a more meaningful warning.

### Options / Alternatives
- **Use only location-based alerts near roadside**  
  For example, proximity warning when a child is near the roadside
- **Incorporate behaviour-based detection when near roadside**  
  For example, detecting sudden acceleration or a sharp change in speed over a short period of time
- **Use advanced ML prediction models to observe and predict child pedestrian behaviour**  
  For example, cameras or nearby infrastructure monitoring a child’s movement patterns

### Evaluation Criteria
- Ability to address unpredictability
- Improvement in early risk detection
- Feasibility of implementation
- Suitability within the intended V2P scope

### Decision and Rationale
The team decided to incorporate **behaviour-based detection specialised for child movement patterns**, with the main focus on identifying sudden acceleration or rapid movement, such as running or sprinting, as indicators of potentially risky behaviour.

This option was selected because it addresses the core issue of unpredictability more directly than location-based alerts alone. A location-based warning may still be useful, but by itself it may not be sufficient, as it could produce too many false positives in roadside environments where children may be present without actually intending to enter the road.

The use of advanced machine learning prediction models was also considered, but it was not chosen as the main approach at this stage. One reason is that machine-learning methods based purely on visual observation, such as camera-based monitoring from nearby infrastructure, may not be sufficient on their own to reliably capture child intention or sudden behaviour in all situations. They may also require large and relevant datasets for training while introducing additional system complexity and cost if cameras or fixed infrastructure are needed.

In addition, there are many different roads, so it is not feasible to install cameras everywhere for constant road monitoring. Such systems would also require a large amount of training data to perform reliably and may still experience blind spots or limited visibility due to the fixed position of cameras.

However, the team acknowledged that machine learning may still be a useful supporting approach in the future, particularly if applied to sensor-based data rather than visual data alone. For example, motion-related data from wearable or mobile sensors could potentially be used to improve prediction of whether a child is likely to start running or suddenly enter the road, together with GPS to determine whether the situation is high risk.

Overall, behaviour-based detection was considered the most appropriate primary option at this initial stage of development, especially since there would not be enough collected data to train a robust model properly. It remains more feasible within the project scope and offers a more focused way to respond to sudden child movement in blind-spot scenarios.

### AI Usage
NA

### Team Members
- Wen Hui
- Khee Xuan
- Yuankai
- Daniel
- Zelda
- Syaakir

</details>

---

<details>
<summary><strong>Decision Log #7 — Selecting the Trigger Logic</strong></summary>

### Date
18/03/2026

### Trigger / Problem
After identifying child pedestrians’ unpredictable movement as the key behaviour to address, the next step was to determine when the system should trigger an alert. Although the system is intended to improve safety by warning vehicles of potentially dangerous child movement, not every movement near a roadside should automatically be treated as high risk.

Children may walk, stop, turn, or play near roadside areas without actually entering the path of a vehicle, so a warning mechanism that is too sensitive could result in frequent unnecessary alerts.

This created an important design challenge for the team. On one hand, the system should provide warnings early enough to give drivers or nearby vehicles more time to react, especially in blind-spot situations where visual awareness may already be limited. On the other hand, if alerts are triggered too easily or too often, the system may produce too many false positives. This could reduce trust in the warning system, cause alert fatigue, and make drivers more likely to ignore or undervalue future warnings.

Therefore, the team needed to evaluate what type of trigger logic would be most suitable for distinguishing normal child movement from genuinely risky behaviour in a way that is both practical and reliable for real-world use.

### Options / Alternatives
- **Always-on proximity alerts**  
  Trigger an alert whenever a child is detected near the roadside
- **Simple threshold trigger**  
  Trigger an alert when speed exceeds a certain value
- **Event-based trigger**  
  Trigger an alert when sudden acceleration or direction change is detected
- **Continuous alert updates**  
  Constantly send updates to nearby vehicles in real time
- **Multi-condition trigger**  
  Combine movement behaviour, proximity, and contextual factors before issuing an alert

### Evaluation Criteria
- Accuracy in detecting risky behaviour
- False positive rate
- System responsiveness
- Whether the trigger condition is valid and applicable for the selected blind-spot child pedestrian scenario

### Decision and Rationale
The team decided to implement a **multi-condition trigger mechanism**, where an alert is issued only when sudden movement, such as rapid acceleration or a sharp change in direction, occurs while the child is near the roadside or within close vehicle proximity.

This option was selected because it provides a more contextual and reliable way to identify risky behaviour compared to using only one condition.

Always-on proximity alerts were not chosen because they are likely to produce a high number of false positives, as a child may be near the road without actually being in immediate danger. A simple threshold trigger based only on speed was also considered insufficient, since it does not account for the surrounding situation or whether the movement is actually hazardous. Continuous alert updates were not selected as the main approach because constantly transmitting warnings may be unnecessary for a real-time safety system and could reduce the clarity and usefulness of alerts.

Overall, the multi-condition trigger was considered the most suitable option because it improves the relevance of warnings, reduces unnecessary alerts, and supports earlier detection of genuinely risky behaviour, making the system more practical and trustworthy for real-world use.

### AI Usage
NA

### Team Members
- Wen Hui
- Zelda
- Syaakir

</details>

---

<details>
<summary><strong>Decision Log #8 — Selecting the Communication Method</strong></summary>

### Date
18/03/2026

### Trigger / Problem
From Decision Log #7, the team had already identified that the system would require a trigger mechanism to determine when risky child movement should generate an alert. The next step was to determine how this alert data should be communicated from the child pedestrian device to nearby vehicles.

To support this discussion, the team made two working assumptions for the concept stage. First, it was assumed that most children in urban areas would have access to a smartphone or a smart wearable device, since such devices are commonly used by parents for communication and safety purposes. Second, it was assumed that the selected use case would take place in an area with sufficient communication coverage for nearby devices to exchange data.

Based on these assumptions, the team considered several communication options supported by smartphones, such as Bluetooth, Wi-Fi, and cellular communication. IEEE 802.11p / DSRC was not considered feasible for direct V2P communication in this case because commercial smartphones do not normally support it directly and it generally requires dedicated vehicular hardware. Existing V2P literature by Sewalkar et al. notes that smartphones commonly provide cellular, Wi-Fi, and Bluetooth connectivity, while 802.11p requires dedicated hardware not generally available on smartphones.

### Options / Alternatives
- Wi-Fi-based V2P communication with compact high-priority safety messages
- Cellular-based V2P communication with compact high-priority safety messages
- Bluetooth Low Energy (BLE)-based V2P communication with compact high-priority safety messages

### Evaluation Criteria
- Communication latency
- Reliability under short-range urban conditions
- Packet loss / interference sensitivity
- Feasibility with standard smartphones
- Suitability for safety-critical alerts
- Fit with the proposed blind-spot child pedestrian scenario

### Comparison Summary

| Communication Method | Latency | Robustness (BER) | Power Consumption | Coverage |
|---|---:|---:|---:|---:|
| Cellular (URLLC) | < 1 ms | > 10^-5 | High | High |
| Bluetooth Low Energy | 7.5 ms – 45 ms | 10^-5 (at low throughput, < 50 bytes) | Low | Medium |
| Wi-Fi | 10 ms – 45 ms | 10^-5 | Medium | Low |

### Decision and Rationale
The team decided to prioritise **BLE-based V2P communication with compact high-priority safety messages** for further concept development.

This option was considered the most suitable at this stage because the communication requirement was mainly for a short-range and time-sensitive first alert to a nearby vehicle, rather than continuous data exchange over a wider area. Compared to Wi-Fi and cellular-based communication, BLE was seen as a more practical option for short-range communication using current smartphone capabilities, given its low power consumption and acceptable latency and robustness for smaller payloads.

BLE was preferred over Wi-Fi because Wi-Fi-based V2P systems typically require association procedures, which can delay the actual exchange of safety messages under mobility. The V2P survey also notes that Wi-Fi range may be sufficient in urban areas, but its association requirement is a key challenge for moving vehicles. BLE was preferred over cellular because cellular communication depends more heavily on network infrastructure and may introduce additional end-to-end delay and variability from the wider network path, even though it offers good range and mobility support.

BLE is not inherently limited to only very short ranges, but its practical range and latency depend strongly on device capability, configuration, and environmental conditions. In some V2P-related studies, Bluetooth-based communication around 50 m has been reported, while Bluetooth LE in general can support much longer ranges under favourable conditions. Likewise, BLE latency can be low enough for time-sensitive alerts, around 7.5 ms to 45 ms, but the actual delay is affected by parameters such as advertising interval, scan settings, and smartphone implementation.

For this reason, the team did not treat BLE as a guaranteed long-range or fixed-latency solution. Instead, it was considered suitable mainly for delivering a short-range first alert to a nearby vehicle, where its support on common smartphones and ability to transmit lightweight safety messages made it a practical option for further concept development. This choice is also supported by more recent work that implemented smartphone-based vehicle-to-VRU communication using BLE advertising specifically to warn nearby vehicles in partially occluded or occluded situations.

The transmitted message should be kept small and safety-prioritised, containing only essential data such as estimated location, movement intensity or sudden acceleration status, and alert level. This reduces communication overhead, lowers the chance of congestion-related delay, improves BER performance, and makes the warning mechanism more appropriate for time-critical safety use.

Overall, BLE with compact priority messages was considered the best fit for the proposed system because it does not require additional hardware attachment for current smart devices, with its low power consumption, devices can last longer and it aligns well with the concept’s two-layer design in which a nearby vehicle first receives the child alert and then forwards it to other vehicles.

### AI Usage
Used AI to help compare between the different communication options on their pros and cons to aid in decision making

### Team Members
- Daniel
- Khee Xuan
- Yuankai

</details>

---

<details>
<summary><strong>Decision Log #9 — Selecting the Alert Message Contents</strong></summary>

### Date
19/03/2026

### Trigger / Problem
After deciding on the communication method for transmitting alerts, the next step was to determine what information should be included in the alert message. The main challenge was to ensure that the transmitted message contained enough information for nearby vehicles to recognise the level of risk and respond appropriately, while still keeping the message lightweight so that it could be delivered quickly without introducing unnecessary communication delay or network congestion.

### Options / Alternatives
- **Basic message**  
  Includes the child pedestrian’s estimated location and movement intensity / sudden acceleration status
- **Extended message**  
  Includes the child pedestrian’s estimated location, movement intensity / sudden acceleration status, estimated direction of movement, and timestamp
- **Full-context message**  
  Includes the child pedestrian’s estimated location, movement intensity / sudden acceleration status, estimated direction of movement, timestamp, and additional historical movement or prior sensor data
- **Priority safety message**  
  Includes only essential safety-related information, such as estimated location, movement intensity / sudden acceleration status, and a warning message marked with a priority or alert flag for urgent forwarding

> In this context, “speed” refers more broadly to movement intensity or sudden acceleration status, rather than precise pedestrian walking or running speed.

### Evaluation Criteria
- Message size and transmission latency
- Usefulness for vehicle risk assessment
- Reliability of included data
- Suitability for real-time safety communication

### Decision and Rationale
The team decided to use a **priority safety message structure**, containing only essential safety-related information such as the child pedestrian’s estimated location, movement intensity or sudden acceleration status, and a warning message marked with a priority or alert flag for urgent forwarding.

This option was selected because it provides nearby vehicles with sufficient information to recognise that a potentially dangerous situation may be occurring, while keeping the transmitted message lightweight for faster delivery and lower communication overhead.

The minimal message option was not chosen because location alone may not provide enough context for a vehicle to determine whether the child is actually at immediate risk. The basic message was considered more useful, but location and movement intensity alone may still be insufficient for prioritising urgent alerts within the network. The extended message option was also considered, but estimated direction of movement may not always be reliable in this concept because the smartphone may be carried in different ways, such as in a pocket or bag, which can affect the consistency of directional estimation.

The full-context message was not selected because including historical movement or prior sensor data would increase message size, transmission overhead, and latency, making it less suitable for time-critical safety communication.

Overall, the priority safety message was considered the most appropriate option because it balances practical risk-related information with low communication overhead, allowing alerts to remain fast, focused, and more suitable for real-time warning in the selected blind-spot child pedestrian scenario. At this stage, this decision helped establish the core form of the proposed alerting approach.

### AI Usage
NA

### Team Members
- Wen Hui
- Khee Xuan

</details>

---

<details>
<summary><strong>Decision Log #10 — Selecting the Child Identification Method</strong></summary>

### Date
20/03/2026

### Trigger / Problem
An oversight identified from Decision Log #8 was that a smartphone by itself does not indicate whether the user is a child or an adult. This creates a problem for the proposed system, since the concept is specifically designed around child pedestrian safety and child-related unpredictable movement behaviour. If the pedestrian identity is unknown, the system may not be able to determine whether the alert should be processed under the child-focused warning logic.

As a result, the team needed to evaluate how the system could more reliably determine that the pedestrian involved is a child. A possible solution would be to use a device or platform that requires prior registration, setup, or age-related input. This would allow the system to associate the transmitted alert with the intended user group and improve the integrity and relevance of the warning process.

### Options / Alternatives
- **Watch-based identification**  
  Use a smartwatch or wearable device associated with the child user profile  
  Could potentially leverage an existing device such as a health or fitness tracker given out by the Health Promotion Board
- **Tag-based identification**  
  Use a simple dedicated tag assigned to the child  
  Could act as a lightweight identifier linked to a registered child profile
- **App-based identification**  
  Use a smartphone application with prior registration and age input  
  The app would identify the user as a child based on stored profile details
- **Combined wearable + app approach**  
  Use both a wearable or tag and a smartphone app together  
  The wearable provides a dedicated child-linked identifier, while the app handles registration and supporting sensor data

### Evaluation Criteria
- Reliability in identifying the user as a child
- Feasibility of implementation
- Ease of setup and use
- Suitability for everyday real-world use
- Compatibility with the proposed V2P concept

### Decision and Rationale
The team decided to adopt a **combined wearable + app approach**.

This option was selected because it provides a more reliable way of identifying the pedestrian as a child while also supporting the broader system concept. The app can handle initial registration and age-related profile information, which can be set up by parents or guardians, while the wearable or tag can act as a more dedicated child-linked identifier during actual use. This makes the identification process more robust than relying only on a smartphone, which by itself does not reveal whether the user is a child.

A watch-based identification option was considered useful because it is wearable and is more likely to remain with the child consistently during daily use. In addition, if the watch is a smartwatch, it may also be capable of collecting relevant motion-related sensor data, transmitting safety-related information, and potentially working together with a supporting mobile application. This makes it more than just an identification device, as it could also contribute to sensing and communication within the wider concept.

An app-based identification approach was likewise considered feasible, as a smartphone application can support user registration, profile management, and age-related setup. However, this option depends strongly on the assumption that the child is consistently carrying and using the correct device, which may reduce its reliability in real-world use.

Overall, the combined wearable + app approach was considered a useful refinement to the emerging proposed concept, as it improves the reliability of identifying whether the pedestrian is a child while remaining practical and compatible with the wider system design. This decision strengthened the concept by providing a clearer way to distinguish child users from general pedestrians.

### AI Usage
NA

### Team Members
- Zelda
- Daniel
- Syaakir

</details>

---

<details>
<summary><strong>Decision Log #11 — Extending the Warning Path with V2V Relaying</strong></summary>

### Date
22/03/2026

### Trigger / Problem
Another issue identified after Decision Log #8 was that, although BLE appeared suitable for short-range smartphone-based V2P communication due to its practicality, low power usage, and support on common smartphones, the direct child-to-vehicle link alone might not be sufficient in blind-spot situations. In such cases, not all approaching vehicles may receive the child’s alert early enough, especially if they are farther away, partially occluded, or not yet within effective first-hop communication range.

For this stage of concept development, the team also assumed that vehicles within the selected area or scenario would be capable of receiving BLE-based alerts from the child pedestrian device, and that vehicles would also be able to communicate with one another through a suitable V2V communication method. This assumption was necessary in order to evaluate whether a layered warning approach could be feasible in the selected scenario.

At the same time, DSRC / IEEE 802.11p is commonly associated with low-latency V2V safety communication, but it is generally not supported directly by smartphones. After reviewing the lecture slides on DSRC (IEEE 802.11p), the team noted that DSRC operates at low transmission latency. Nevertheless, it was also recognised that future developments in V2X-capable mobile devices, 5G-based C-V2X, or sidelink communication may make direct low-latency pedestrian-device communication more feasible in later implementations.

Hence, the team considered whether a second safety layer could be introduced, whereby the vehicle closest to the child pedestrian first receives the safety alert and subsequently relays the warning to other approaching vehicles through low-latency V2V communication. Such an approach could improve warning propagation in situations where a child suddenly runs into traffic and vehicles further away may not yet have direct awareness of the risk. The team therefore needed to evaluate whether a combined V2P and V2V communication approach would be more suitable than relying solely on a direct child-to-vehicle alert.

### Options / Alternatives
- **Infrastructure-based communication then to vehicles**  
  The child pedestrian’s alert is first transmitted through roadside infrastructure, such as an RSU or cloud-based system, before being forwarded to vehicles.
- **Combination of V2P and V2V communication**  
  The child pedestrian’s device sends the initial safety alert to a nearby vehicle, which then relays the warning to other approaching vehicles through V2V communication as a multi-hop message.

### Evaluation Criteria
- Latency of message delivery
- Warning coverage range
- Reliability in blind-spot situations
- Dependency on external infrastructure
- Feasibility with current smartphone limitations
- Suitability for safety-critical communication

### Decision and Rationale
The team decided to prioritise a **combination of V2P and V2V communication** for further concept development.

This option was considered more suitable because it allows the initial alert to be transmitted directly from the child pedestrian device to a nearby vehicle using smartphone-supported V2P communication, while also extending the warning to other approaching vehicles through low-latency V2V relaying.

Direct V2P communication alone was not chosen as the only approach because its effectiveness may be limited by first-hop range, occlusion, and the possibility that not all relevant vehicles will receive the alert early enough in a blind-spot scenario. In particular, while the nearest vehicle may still receive the warning, vehicles further away or with less direct awareness of the child pedestrian may not be alerted in sufficient time if the system depends only on a direct child-to-vehicle link.

In comparison, the combined approach was considered more effective because it improves both initial detection and warning propagation. Once the nearest vehicle receives the child’s alert, it can act as a relay to forward the warning to other approaching vehicles, helping to extend coverage beyond the original first-hop communication range. This was seen as especially relevant in the selected blind-spot child pedestrian scenario, where some drivers may not yet have direct visibility of the child or awareness of the risk.

Overall, the combined V2P and V2V approach was considered an important refinement to the concept, as it enhances the warning pathway beyond direct child-to-vehicle communication alone. This decision improved the robustness of the proposed system by extending alert propagation to other approaching vehicles, particularly in blind-spot scenarios where direct awareness may be limited.

### AI Usage
NA

### Team Members
- Yuankai
- Daniel
- Khee Xuan
- Zelda
- Syaakir
- Wen Hui

</details>

---

<details>
<summary><strong>Decision Log #12 — Selecting the Wearable Device Type</strong></summary>

### Date
24/03/2026

### Trigger / Problem
Based on Decision Log #10 and its evaluation, the next step was to determine which type of wearable device would be most suitable for integration into the proposed system. This became an important consideration because, although the team had already identified the value of using a wearable to improve child-user identification and system reliability, the exact form of the wearable had not yet been decided.

Different wearable form factors may provide different strengths and limitations in terms of sensor capability, communication support, usability, comfort, and practicality for children. Some devices may be more suitable for continuous wearing and easier for children to keep with them consistently, while others may offer better sensing or communication functions but be less convenient for everyday use. Since the proposed concept depends not only on identifying the user as a child, but also potentially supporting motion detection and transmission of alert-related data, the choice of wearable could have a direct impact on the overall feasibility and effectiveness of the system.

In addition, the team also needed to consider whether the wearable should play a more active role in the wider system by contributing sensor data and supporting communication with the smartphone, application, or nearby vehicles. Therefore, this decision was necessary to refine the concept further and ensure that the selected wearable type would be compatible with both the intended user group and the broader system design.

### Options / Alternatives
- **Wearable Tags (clip-on / bag-based)**  
  - Long battery life  
  - Discrete and lightweight  
  - Low cost and scalable  
  - Can capture GPS and basic motion
- **Neck-worn Trackers**  
  - Long battery life  
  - Compatible with apps  
  - Can capture GPS and basic motion  
  - Includes safety features such as SOS alerts and geofencing
- **Wrist-based Trackers**  
  - Discrete and hard to remove  
  - Ensures consistent tracking as it is always worn  
  - Can capture GPS and basic motion
- **Smartwatch**  
  - Able to capture rich sensor data  
  - Enables detection of behavioural patterns  
  - Supports real-time communication  
  - Compatible with apps

### Evaluation Criteria
- Ability to capture rich sensor data
- Compatibility with apps for configuration and integration
- Ease of use and setup for both children and guardians
- Comfort and wearability for everyday use
- Suitability for real-world deployment in child safety scenarios

### Decision and Rationale
The team decided to proceed with the **smartwatch-based wearable** as the primary device.

This decision was made because the smartwatch provides the most comprehensive set of capabilities aligned with the system’s objectives. It is able to capture rich, multi-dimensional sensor data, which is important for analysing and interpreting the child’s movement and behaviour in real time.

Compared to the other options, wearable tags and wrist-based trackers were recognised as practical, lightweight, and potentially more energy-efficient, but they were considered more limited in functionality, as they generally focus on basic location tracking and simple motion data. While these features may support identification and basic monitoring, they were seen as less suitable for capturing the richer movement-related information needed for the wider concept. Neck-worn trackers were also considered useful because they may include additional safety-related functions such as SOS alerts and geofencing, but they were similarly viewed as more limited in sensing capability compared to a smartwatch.

The smartwatch was further seen as advantageous because it remains wrist-based, allowing relatively consistent physical contact with the child during everyday use. This may support more stable and reliable data collection compared to wearable forms that may be removed, loosely attached, or carried separately. In addition, a smartwatch can support real-time communication and integrate more easily with a companion mobile application, which fits well with the broader system architecture discussed in earlier logs. Through app integration, functions such as geofencing may also be supported, which could help provide additional location-based context and improve how proximity to roadside risk areas is interpreted. Since the proposed concept also considers BLE-based communication, the smartwatch was seen as a practical device type that could operate effectively within a low-power communication framework.

Therefore, the smartwatch was determined to be the most suitable option as it not only supports reliable child identification, but also enhances the system’s ability to capture, analyse, and respond to child movement behaviour in a meaningful and practical way. As a refinement to the existing concept, the smartwatch can act as a more capable platform for strengthening sensing, communication, and context awareness within the overall system.

### AI Usage
NA

### Team Members
- Zelda
- Yuankai
- Khee Xuan

</details>

---

<details>
<summary><strong>Decision Log #13 — Evaluating Cost and Deployment Feasibility</strong></summary>

### Date
25/03/2026

### Trigger / Problem
As the system concept became more defined, there was a need to evaluate the practical cost and feasibility of deploying the proposed system. While the technical decisions had been largely established, there had been no detailed consideration of the cost implications of the required hardware and software components, nor the broader costs associated with deployment and ongoing maintenance.

This was especially necessary to ensure that the proposed system remains realistic and scalable for real-world usage.

### Options / Alternatives
- **Potential needed hardware costs**
  - Smartphone: assumed already owned by family, no additional procurement cost
  - BLE tag: estimated SGD20–40 per unit (retail), bill of materials USD5–11 per unit
  - Smartwatch: estimated SGD50–150 per unit (retail), bill of materials USD20–50 per unit

- **Potential needed software costs**
  - App development: custom V2P safety application with motion detection, BLE advertising, and alert transmission logic
  - App updates and maintenance: ongoing OS compatibility patches, bug fixes, feature updates
  - Licensing:
    - Apple App Store: USD99/year
    - Google Play: estimated USD25 one time
    - BLE stack licensing: estimated USD1–3 per unit royalty
    - Watch platform SDK fees: estimated USD500–2000 depending on platform
  - Cloud/backend hosting: estimated USD10–50/month at small scale if server-side components are used
  - Map/location API fees: estimated USD7 per 1000 requests beyond free tiers
  - Data privacy and compliance: possible legal consultation or auditing cost if child-related data is collected and stored
  - Push notification services: often free at low volume, but may incur costs at scale

- **Other costs**
  - Manufacturing and factory costs: PCB assembly, enclosure moulding, production at scale for tag/smartwatch
  - Deployment costs: physical distribution and registration of wearable
  - Maintenance costs: tag battery replacement every 6–12 months, smartwatch charging and firmware updates, smartphone app updates

### Evaluation Criteria
- Unit cost and total deployment cost
- Bill of materials and manufacturing feasibility at scale
- Software development and licensing costs
- Ease of distribution and setup
- Ongoing maintenance requirements

### Decision and Rationale
For hardware, the smartphone was assumed to already be owned by the family, meaning no additional procurement or manufacturing cost would be incurred for this component. The application would handle motion data collection and BLE-based V2P alert transmission. Since modern mid-range smartphones, estimated at around SGD300–500, already support BLE and the required sensors, this component was considered practically accessible for many families in Singapore.

For the wearable, the team evaluated two sub-options. A dedicated BLE tag, similar to Tile trackers or Apple AirTag, adds only a minimal incremental hardware cost, with bill of materials estimated at approximately USD5–11 per unit, comprising a BLE SoC such as the Nordic nRF52 series at roughly USD2–4. At retail, this translates to an estimated low unit cost of approximately SGD20–40, which is highly affordable and scalable. At larger production volumes, unit manufacturing costs would decrease further due to economies of scale, improving overall cost viability. A children’s smartwatch was considered as an alternative wearable, but its higher bill of materials of approximately USD20–50 per unit and retail price of SGD50–150 per unit make it comparatively more expensive.

For software, the primary development effort is concentrated in the smartphone application which handles motion data, child identification logic, and BLE alert transmission. An open-source BLE stack such as Zephyr RTOS is available at no licensing cost, avoiding per-unit royalty fees associated with proprietary stacks. App store distribution fees are minimal, limited to the Apple Store annual fee of approximately USD99 and a one-time Google Play fee of USD25. If the smartwatch option is adopted, additional platform SDK licensing costs of approximately USD500–2000 may apply depending on the watch platform. Ongoing software maintenance costs are expected to be low to moderate, mainly consisting of periodic application updates for OS compatibility and bug fixes.

For deployment and manufacturing, no fixed infrastructure installation is required, which helps keep deployment costs low. A tag requires physical distribution and initial registration by a parent or guardian, which introduces some logistics cost. At manufacturing level, the tag’s simple construction consists mainly of a BLE SoC, PCB, battery, and enclosure, whereas the smartwatch requires additional components such as a display, GPS module, and integrated sensors. Ongoing maintenance is similarly low for the tag option, limited primarily to battery replacement approximately every 6–12 months depending on the BLE advertising interval, together with periodic application updates.

Overall, the discussion highlighted that the concept remains feasible from a cost perspective, especially if it leverages already-owned smartphones and avoids extensive infrastructure deployment. However, the choice between a tag and a smartwatch would involve a trade-off between lower cost and richer sensing capability.

### AI Usage
Use AI to help estimate the total cost of the proposed solution in overall

### Team Members
- Wen Hui
- Khee Xuan
- Zelda

</details>

---

<details>
<summary><strong>Decision Log #14 — Selecting the Driver Alert Delivery Method</strong></summary>

### Date
25/03/2026

### Trigger / Problem
Alerts can be further relayed between vehicles through V2V communication such as DSRC to extend warning coverage. However, there was still a need to determine how the driver should receive and interpret these alerts once the vehicle receives them. The team therefore considered whether alerts should be delivered through the driver’s smartphone or directly through the vehicle’s OBU.

In addition, the team needed to assess how the alert should be presented to the driver so that it is both noticeable and effective, particularly in blind-spot situations where reaction time is limited. The system must also account for whether the alert itself may startle the driver, as this could either improve reaction time or negatively affect the driver’s response.

### Options / Alternatives
- **Smartphone-based alert**
  - Easier to implement
  - No need for special vehicle hardware
  - Works with existing phones
- **OBU visual alert**
  - Clear and informative
  - Device is already right in front of the driver’s view
  - Not too disruptive
- **OBU audio alert**
  - Grabs attention immediately
  - Good for urgent situations
- **Combination of visual and audio alerts through OBU**
  - Driver may react faster with audio alert
  - Clear warning shown on the OBU
  - More realistic for a safety application

### Evaluation Criteria
- Driver reaction time
- Clarity and visibility of the alert
- Ease of interpretation while driving
- Feasibility of implementation

### Decision and Rationale
Based on the team’s discussion, it was decided that the vehicle’s **OBU** would be prioritised as the primary method for alerting the driver. Compared to smartphone-based alerts, the OBU was considered more reliable because it is directly integrated into the vehicle environment, making it more likely that the driver will notice warnings presented through the dashboard, head unit, or another in-vehicle interface.

At this stage, the team assumed that the vehicle would be equipped with a system capable of receiving BLE-based alerts from nearby devices, and that these alerts could be passed to the vehicle’s OBU or dashboard system for driver notification. This assumption allowed the concept to remain compatible with smartphone-based V2P communication while still making use of a more practical in-vehicle warning mechanism.

The team also decided to use a **combination of visual and audio alerts**. A visual warning would provide clear on-screen information to the driver, while a controlled audio cue would help attract immediate attention. However, the audio alert should be designed carefully so that it is noticeable without being overly abrupt or startling, as excessive alarm intensity may negatively affect driver response.

Overall, the use of the OBU was considered a useful refinement to the concept because it improves how the warning is delivered to the driver after the alert has already been transmitted and received.

### AI Usage
NA

### Team Members
- Yuankai
- Syaakir
- Daniel

</details>

---

<details>
<summary><strong>Decision Log #15 — Adding Geofencing to Improve Location Context</strong></summary>

### Date
26/03/2026

### Trigger / Problem
At this stage of development, the team was operating under the assumptions that the child would be wearing a BLE-capable smartwatch, that the target use case would occur primarily on low-speed urban or residential roads, and that forwarded alerts would be limited by time and hop count to control network congestion. Within these assumptions, the current concept was relying mainly on motion data from the smartwatch to detect sudden movement changes and trigger alerts to nearby vehicles.

However, the team realised that this motion-based approach alone is not sufficient to determine the child’s actual location context. As a result, the system cannot distinguish whether the child is genuinely approaching the road, waiting near a blind spot or hazardous crossing area, or simply moving quickly in a non-dangerous location. This may cause the application to repeatedly send unnecessary alerts whenever the motion threshold is exceeded, increasing the likelihood of false positives and reducing the reliability of the system.

### Options / Alternatives
- Add a manual user trigger to activate alerts only when needed
- Implement geofencing so that motion-based alerts are only triggered when the child is within predefined blind-spot or danger zones
- Combine geofencing with motion data and direction analysis for more context-aware alert triggering

### Evaluation Criteria
- Ability to reduce false alerts
- Ability to identify whether the child is in a hazardous roadside area
- Practicality of implementation on a smartwatch
- Reliability for real-time warning scenarios
- Suitability for blind-spot and danger zone use cases

### Decision and Rationale
The selected solution was to implement **geofencing together with the existing motion-based detection**.

In this approach, predefined blind spots or danger zones are first configured in the system using location boundaries. The smartwatch application then continuously checks whether the child is within one of these zones. Only when the child is inside a defined hazardous area will the motion data be evaluated to determine whether an alert should be triggered.

This solution was selected because geofencing adds important location context that is missing in the current motion-only approach. By limiting alert generation to predefined high-risk areas, the system can better distinguish between normal child movement and movement that may actually indicate road-crossing risk. This helps reduce unnecessary warnings, improves system reliability, and makes the alert mechanism more relevant to the intended blind-spot safety scenario.

Overall, the integration of geofencing strengthens the system by ensuring that alerts are triggered based on both **where** the child is and **how** the child is moving, rather than motion alone.

### AI Usage
AI was used to generate ideas on how the team could better determine the actual location of the child.

### Team Members
- Yuankai
- Khee Xuan

</details>

---

<details>
<summary><strong>Decision Log #16 — Selecting the Communication Exchange Method</strong></summary>

### Date
26/03/2026

### Trigger / Problem
To ensure the communication process is efficient and suitable for the intended safety application, the team needed to determine what type of communication exchange method would best support the system. Since the concept is designed to send urgent alerts to nearby vehicles in time-critical situations, this was important to ensure that the communication process remains reliable, efficient, and appropriately aligned with the specific challenges of the child pedestrian safety scenario being addressed.

### Options / Alternatives
- Handshake protocol
- Broadcast protocol
- Scan response
- Pairing
- DSRC

### Evaluation Criteria
- Ensure optimal and efficient communication between the devices based on system requirements
- Suitability for time-critical safety communication
- Communication efficiency

### Decision and Rationale
The **broadcast protocol** was selected as the most suitable communication approach for this system.

Smart devices would utilise broadcasting to transmit messages to nearby vehicles, ensuring that the closest vehicles receive the alert with minimal delay. Once a vehicle receives the message, it can then leverage DSRC to propagate the information to following vehicles, enabling efficient and timely dissemination of warnings along the traffic flow.

For scan response, the team noted that there was no need for the receiving vehicle to request additional data, since the communication is intended to be primarily one-way, with vehicles simply receiving the alert and taking note of the situation. Therefore, including scan response was considered unnecessary for the intended application.

The team also considered whether the system required the receiver to acknowledge the transmitted message. For this use case, acknowledgement and pairing were not considered necessary. If a nearby vehicle fails to receive the initial broadcast, establishing a handshake or pairing would not be practical, as the broadcasting device is expected to continue transmitting the message repeatedly for a limited period. Attempting to perform a handshake or pairing would introduce additional delay, which would be undesirable in situations where the child may already be at immediate risk. Furthermore, if the receiver is unable to receive the message or pairing fails, the sender has limited recovery options beyond retransmitting it. In this context, continuous broadcasting was considered the more effective approach.

The main drawback, however, is that repeated broadcasting may increase channel congestion, especially in environments where there are many nearby devices or vehicles. This was recognised as an important limitation to take note of.

### AI Usage
AI was used to generate alternative options besides broadcasting and handshake protocol.

### Team Members
- Khee Xuan
- Daniel

</details>

---

<details>
<summary><strong>Decision Log #17 — Controlling Alert Lifetime and Relay Range</strong></summary>

### Date
26/03/2026

### Trigger / Problem
To address congestion, the system must define a time threshold after which the child is no longer classified as an immediate threat, and also determine the range within which the vehicle is required to relay its messages.

Following the implementation of V2P and V2V communication, the team identified a potential issue regarding message propagation and network congestion. If alerts are continuously relayed between vehicles without limitation, it may result in unnecessary network overload and outdated warnings being transmitted.

Therefore, the team had to determine how long a child pedestrian should be considered a threat after the initial detection, as well as the range within which vehicles should continue relaying the alert message.

### Options / Alternatives
- **Fixed time threshold**  
  Alerts expire after a set duration
- **Distance-based threshold**  
  Alert is only relayed within a certain range
- **Hop-based limitation**  
  Limit the number of times a message can be forwarded between vehicles
- **Combination of both time and distance-based control**

### Evaluation Criteria
- Evaluation would be based on what is needed for the system
- Ensuring optimal communication between the devices

### Decision and Rationale
The team decided to use a **fixed timeout threshold of 0.5 seconds** and a **distance-based threshold of 50 metres** for DSRC communication between vehicles.

The 0.5-second timeout was chosen because any delay longer than this may reduce the usefulness of the warning. After around 0.5 seconds, the driver would likely have already seen the pedestrian or hazard approaching, and any alert sent after that point could be too late to support a meaningful reaction. Therefore, a shorter timeout helps ensure that the warning remains relevant and timely in a safety-critical situation.

The 50-metre distance threshold was selected to limit the communication range to only nearby vehicles that need to be aware of the situation. This helps focus the DSRC system on vehicles within a relevant proximity, instead of notifying vehicles that are too far away for the warning to be useful. By restricting the range to 50 metres, the system can reduce unnecessary alerts and make the communication more targeted and practical for immediate road safety purposes.

Overall, congestion is reduced by limiting BLE communication to only a 0.5-second timeout threshold. The short timeout ensures that only timely and useful warnings are transmitted, instead of outdated messages that may no longer help the driver. At the same time, the 50-metre range restricts alerts to nearby vehicles, preventing unnecessary message broadcasting to vehicles that are too far away to be affected. As a result, the system reduces communication overload, improves the relevance of alerts, and helps prevent network congestion while still maintaining effective safety awareness among nearby vehicles.

### AI Usage
NA

### Team Members
- Khee Xuan
- Wen Hui
- Yuankai
- Zelda

</details>

---

<details>
<summary><strong>Decision Log #18 — Identifying Key System Limitations</strong></summary>

### Date
27/03/2026

### Trigger / Problem
As the concept became more developed, the team recognised that the proposed system may still face several practical limitations in real-world operation. Since the solution depends on sensor readings, wireless communication, and timely alert delivery, its effectiveness could be affected by signal loss, communication breakdown, packet delay, or inaccurate sensing of child movement. It was therefore important to identify the main limitations of the concept and reflect on how these issues could influence the overall feasibility, safety, and robustness of the system.

### Options / Alternatives
- **Communication failure or signal loss**  
  Alerts may not be transmitted successfully due to weak BLE connection, interference, or unstable network conditions
- **Sensor inaccuracy or incorrect motion detection**  
  Smartphone or wearable sensors may misinterpret normal movement as risky behaviour, or fail to detect actual sudden movement accurately
- **Message delay or packet loss**  
  Alert data may arrive too late, incompletely, or not at all, reducing the usefulness of the warning in time-critical scenarios
- **Environmental limitations of BLE communication**  
  Outdoor conditions, obstructions, interference, and variable device placement may affect BLE performance and reliability

### Evaluation Criteria
- Impact on system safety
- Effect on warning reliability
- Likelihood of occurrence in real-world use
- Severity of consequences if unaddressed
- Influence on the overall feasibility and robustness of the concept

### Decision and Rationale
The team concluded that **all of the identified limitations are important** and collectively shape the feasibility and robustness of the proposed concept. Rather than treating them as isolated issues, the team recognised that these limitations are closely related and may affect one another during real-world operation.

Among them, communication failure or signal loss was considered the most critical limitation because the effectiveness of the proposed system depends fundamentally on whether the warning can actually be delivered to a nearby vehicle in time. Even if risky child movement is detected correctly, the system would still fail in its safety purpose if the alert does not reach the intended vehicle. For this reason, communication reliability was seen as the most immediate concern.

At the same time, sensor inaccuracy was also acknowledged as a major limitation because unreliable motion detection could lead to false positives or missed warnings. If normal child movement is incorrectly classified as dangerous, the system may generate unnecessary alerts and reduce user trust. On the other hand, if sudden risky movement is not detected properly, the warning may never be triggered when it is actually needed.

Message delay or packet loss was similarly recognised as an important limitation, especially because the concept is intended for time-critical road safety scenarios. A warning that arrives late may still technically be transmitted, but may no longer be useful enough to help the driver or vehicle respond safely. This means that communication success alone is not sufficient; the timing and completeness of the transmitted message also matter significantly.

The team also noted the environmental limitations of BLE communication, particularly in outdoor roadside scenarios. Factors such as surrounding interference, physical obstructions, body shielding, weather conditions, or inconsistent device placement may all influence how reliably the signal is transmitted and received. Since the concept assumes practical use in real traffic environments rather than controlled indoor settings, this was considered an important limitation affecting overall system consistency.

Overall, this discussion did not result in a single corrective solution, but instead served to document the key limitations that must be acknowledged as part of the concept’s development. While communication failure was viewed as the most critical limitation, the team recognised that sensor accuracy, message delay, and environmental communication constraints all remain important factors that together determine whether the proposed system would be practical, reliable, and robust in real-world application.

### AI Usage
NA

### Team Members
- Yuankai
- Daniel
- Khee Xuan
- Zelda
- Syaakir
- Wen Hui

</details>


# AI Usage

AI Tools Used:
ChatGPT

<details>
<summary><strong>AI Prompts</strong></summary>
AI Prompt 1:

AI Prompt 2:

AI Prompt 3:

</details>

<details>
<summary><strong>Identified Weaknesses/Hallucinations</strong></summary>
AI Weakness 1:
  
AI Weakness 2:

AI Weakness 3:

</details>


# Individual Reflections
<details>
<summary><strong>Yuankai</strong></summary>

</details>
<details>
<summary><strong>Daniel</strong></summary>

</details>
<details>
<summary><strong>Khee Xuan</strong></summary>

</details>
<details>
<summary><strong>Zelda Chua</strong></summary>

</details>
<details>
<summary><strong>Syaakir</strong></summary>
Throughout this project, I was able to work well with my team to develop our V2P system. We contributed together to the different parts of the project, including the decision logs and overall system design. I took part in discussions to better understand how the system works and to help identify any gaps in our idea. There were times where I was unsure about certain concepts, but my teammates helped to clarify them, and I was able to learn from both the project and the team. Overall, this project was a good learning experience for me, as I was able to improve my understanding of the topic while also working collaboratively with my team.
</details>
<details>
<summary><strong>Wenhui</strong></summary>

</details>

# References

References

Sng, E. (2026, February 26). Police Life | Traffic accidents rose in 2025. police.gov.sg. https://www.police.gov.sg/Media-Hub/Police-Life/2026/02/Traffic-Accidents-Rose-in-202

Sewalkar, P., & Seitz, J. (2019, January 17). Vehicle-to-pedestrian communication for Vulnerable road users: Survey, Design Considerations, and challenges. Sensors (Basel, Switzerland). https://pmc.ncbi.nlm.nih.gov/articles/PMC6359035/

Certad, N., Varughese, J., Del Re, E., & Olaverri-Monreal, C. (2025, April 9). V2P Collision Warnings for Distracted Pedestrians: A Comparative Study with Traditional Auditory Alerts. https://arxiv.org/html/2504.13906v1

Gelbal, S. Y., Aksun-Guvenc, B., & Guvenc, L. (2024, January 12). Vulnerable road user safety using mobile phones with vehicle-to-vru communication. MDPI. https://www.mdpi.com/2079-9292/13/2/331

Oh, J., & Kim, J. (2025, February). Potential risk factors of child pedestrian crashes after-school hours in Seoul, Korea. https://www.sciencedirect.com/science/article/pii/S2694610626000160

S, S., V, V., D, S., Priya, D. V., Hamza, M., & Bahl, A. (2025). Enhancement of 5G Ultra-Reliable Low-Latency Communication: Application of Edge Computing and AI-Based Approaches. 2025 IEEE 2nd International Conference on Information Technology, Electronics and Intelligent Communication Systems (ICITEICS), 1–6. https://doi.org/10.1109/iciteics64870.2025.11341410

5G Network Reliability Explained | A10 Networks. (2019, August 5). A10 Networks. https://www.a10networks.com/blog/5g-network-reliability-explained/

Pang, B., Claeys, T., Hallez, H., & Boydens, J. (2024). Modeling the Trade-off between Throughput and Reliability in a Bluetooth Low Energy Connection. ArXiv.org. https://arxiv.org/abs/2405.01231

Apple. (2019). Apple Developer Program - Apple Developer. Apple.com. https://developer.apple.com/programs/

Google Play Console | Google Play Console. (n.d.). Play.google.com. https://play.google.com/console/about/

nRF52840 - Nordic Semiconductor. (n.d.). Www.nordicsemi.com. https://www.nordicsemi.com/Products/nRF52840

The Zephyr Project – A proven RTOS ecosystem, by developers, for developers. (n.d.). Zephyr Project. https://zephyrproject.org/

Get started with Wear OS. (n.d.). Android Developers. https://developer.android.com/training/wearables

Fitbit Development: Fitbit SDK. (n.d.). Dev.fitbit.com. https://dev.fitbit.com/

AWS. (2019). Pricing. Amazon Web Services, Inc. https://aws.amazon.com/pricing/

Google. (2025). GCP Pricing. Google Cloud. https://cloud.google.com/pricing

PDPC. (2025). PDPA Overview. Www.pdpc.gov.sg. https://www.pdpc.gov.sg/Overview-of-PDPA/The-Legislation/Personal-Data-Protection-Act
