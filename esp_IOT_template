#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

int led = D1;
int fan = D5;
const char* ssid = "A12";
const char* password = "12345678";


IPAddress local_IP(192, 168, 50, 70);
IPAddress gateway(192, 168, 50, 1);
IPAddress subnet(255, 255, 255, 0);


ESP8266WebServer server(80);

char index_html[] PROGMEM = R"rawliteral(
<!DOCTYPE HTML>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fotovat</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    overflow: hidden;
      background: linear-gradient(to bottom left, red, black);
      font-family: Arial, sans-serif;
    }
    .button-container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    button {
      height: 150px;
      width: 150px;
      font-size: 24px;
      border-radius: 50%;
      border: none;
      color: white;
      background-color: #FF6F61;
      cursor: pointer;
      transition: background-color 0.7s, transform 0.9s;
      margin: 15px 0;
    }
    button:hover {
      background-color: #553432;
      transform: scale(1.4);
    }
  
    .middle-buttons {
      display: flex;
      justify-content: center;
      gap: 150px;
    }
   
  #star-field {
    position: absolute;
    width: 100vw;
    height: 100vh;
    z-index: -1;
  }

  .star {
    position: absolute;
    border-radius: 50%;
    background: white;
    opacity: 1;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }
  </style>
</head>
<body>
   <div id="star-field"></div>
   <div class="button-container">
     <form action="/Fan_on" method="get" id="form1">
       <button type="submit" value="FanOn">Fan On</button>
     </form>
     <div class="middle-buttons">
       <form action="/Fan_off" method="get" id="form2">
         <button type="submit" value="FanOff">Fan Off</button>
       </form>
       <form action="/LED_on" method="get" id="form3">
         <button type="submit" value="LedOn" id="ledButton">Led On</button>
       </form>
     </div>
     <form action="/LED_off" method="get" id="form4">
       <button type="submit" value="LedOff">Led Off</button>
     </form>
   </div>
   <script>
  const starField = document.getElementById('star-field');
  const numStars = 100;

  // Function to get a random number between min and max
  function getRandomNumber(min, max) {
    return Math.random() * (max - min) + min;
  }

  // Create stars
  for (let i = 0; i < numStars; i++) {
    const star = document.createElement('div');
    star.className = 'star';
    const size = getRandomNumber(1, 5);
    star.style.width = `${size}px`;
    star.style.height = `${size}px`;
    star.style.top = `${getRandomNumber(-10,110)}vh`;
    star.style.left = `${getRandomNumber(-12,110)}vw`;

    // Set random brightness
    star.style.opacity = getRandomNumber(0.5, 1);

    // Randomly decide if the star should blink
    if (Math.random() > 0.7) { // 60% chance to blink
      const animationDuration = getRandomNumber(1, 7);
      star.style.animation = `blink ${animationDuration}s infinite`;
      star.style.animationFillMode = 'forwards';
    }

    starField.appendChild(star);
  }

  // Track mouse movement
  document.addEventListener('mousemove', (event) => {
    const stars = document.querySelectorAll('.star');
    stars.forEach(star => {
      const starRect = star.getBoundingClientRect();
      const starCenterX = starRect.left + starRect.width / 2;
      const starCenterY = starRect.top + starRect.height / 2;

      // Calculate the difference between the star position and mouse position
      const deltaX = (event.clientX - starCenterX) * 0.1; // Adjust multiplier for speed
      const deltaY = (event.clientY - starCenterY) * 0.1; // Adjust multiplier for speed

      // Apply the movement
      star.style.transform = `translate(${deltaX}px, ${deltaY}px)`;
    });
  });
   </script>
</body>
</html>
)rawliteral";
void handleRoot() {
  String mysite = String((char *)index_html);
  server.send(200,"text/html",mysite); 
}
void Function_FanOn(){
    digitalWrite(fan,HIGH);
    Serial.println("Fan on");
    handleRoot();
}
void Function_FanOff(){
    digitalWrite(fan,LOW);
    Serial.println("Fan off");
    handleRoot();
}
void  Function_LEDon(){
    digitalWrite(led,HIGH);
    Serial.println("led on");
    handleRoot();
}
void Function_LEDoff(){
    digitalWrite(led,LOW);
    Serial.println("led off");
    handleRoot();
}
void setup() {
  pinMode(fan, OUTPUT);
  pinMode(led, OUTPUT);

  Serial.begin(115200);

  WiFi.mode(WIFI_AP);
  if (!WiFi.softAPConfig(local_IP, gateway, subnet)) {
    Serial.println("Error while conecctong");
  }

  //Setting up an Access Point
  WiFi.softAP(ssid, password);
  Serial.println("Access Point started");
  
  // Displaying the IP address
  Serial.print("IP Address: ");
  Serial.println(WiFi.softAPIP());

  server.on ( "/", handleRoot );
  server.begin();
  Serial.println("HTTP server started");
  server.on("/Fan_on",Function_FanOn);
  server.on("/Fan_off",Function_FanOff);
  server.on("/LED_on",Function_LEDon);
  server.on("/LED_off",Function_LEDoff);

}

void loop() {
  server.handleClient();
}
