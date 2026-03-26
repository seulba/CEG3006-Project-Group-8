# CEG3006-Project-Group-8


# System Integration

## Overall Solution


## Literature Review

## System Architecture

<img alt="image" src="https://github.com/user-attachments/assets/151f1925-3976-4ca6-bad5-b757e70b650b" width="500"/>

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
<img alt="image" src="https://github.com/user-attachments/assets/6da23dcd-5d3a-47bc-9feb-a32a27ab7322" width="450" />

The system begins by continuously monitoring the child’s location and motion data through the smartwatch. When the smartwatch detects that the child has entered a predefined geofenced blindspot or hazardous roadside area, the accelerometer and gyroscope data is further analysed to determine whether the child is moving toward the road at a potentially dangerous speed or pattern. If both conditions are satisfied, the smartwatch generates and broadcasts a BLE-based warning message to vehicles within a 15metre range of .

A nearby vehicle equipped with a compatible receiver then receives the alert, validates the message, and warns the driver through the vehicle dashboard or head unit. To improve awareness for approaching vehicles, the first receiving vehicle may rebroadcast the message to nearby vehicles within a range of 50metres. The forwarding process is controlled using message expiry time and hop count to prevent excessive rebroadcasting.


## Hardware Components

## Real-life Scenario Use Case


# Decision Logs



# AI Usage



# Individual Reflections



# References
