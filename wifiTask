void WiFiTask(void *pvParameters){
  (void) pvParameters;
  for (;;){
    String message1;
    String message2;
    if(wiFi){
      int art = atoi(::activationWifi.c_str());
      if(art == 2){
        message1 = ">>> Exit Wi — Fi in Send Settings";
        message2 = ">>> Client Timeout in Send Settings!";
        
        String incoming = ParsingParameters();
        ::messageToServer = "AM&" + incoming;
      } else if(art == 0 || art == 1){
        message1 = ">>>  Exit Wi - Fi in WiFI Task";
        message2 = ">>> Client Timeout in WiFi Task!";

        char buffer1[5] = {0};
        std::string str6 = itoa(minuteWeekMain, buffer1, 10);
        char* command = "AB";
        std::string str1(email);
        std::string str2(device_name);
        std::string str5(command);
        std::string finish = str5 + "&" + str1 + "&" + str2 + "&" + str6 + "\r";
        String askServer = finish.c_str();
        ::messageToServer = askServer;
        
      } else {
        message1 = ">>> Exit Wi — Fi in Send Deactivation";
        message2 = ">>> Client Timeout in Send Deactivation!";

        char* command = "AL";
        std::string str1(email);                                                   
        std::string str2(device_name);
        std::string str5(command);
        std::string finish;
        
        if(art == 3){
          finish = str5 + "&" + str1 + "&" + str2 + "&" + "0" + "\r";
        } else if(art == 4) {
          finish = str5 + "&" + str1 + "&" + str2 + "&" + "1" + "\r";
        } else if(art == 5) {
          finish = str5 + "&" + str1 + "&" + str2 + "&" + "2" + "\r";
        }
        
        String askServer = finish.c_str();;
        ::messageToServer = askServer;
      }

      boolean sendOk = false;
      xSemaphoreTake(xMutex, portMAX_DELAY);   

      while(!sendOk){
   
        WiFi.begin(nameWiFi, passwordWiFi);
        int i = 0;

        while(WiFi.status() != WL_CONNECTED) {
          vTaskDelay(100);
          char char2[11];
          snprintf(char2, sizeof char2, "%lu", (unsigned long) i);
          i++;
          if(i == 50){
            Serial.println(message1);
            i = 0;  
            break;
          }
        }

        String line = "";

        if(WiFi.status() == WL_CONNECTED){
          client.setTimeout(3);
          client.connect(host, port);
          vTaskDelay(100);   
          client.print(::messageToServer);
          bool running1 = true;
          unsigned long timeout = millis();
          while (client.available() == 0 && running1) {
            if (millis() - timeout > 2000) {
               Serial.println(message2);
               client.stop();
               running1 = false;
            }
          }
             
          while(client.available()) {
            line = client.readStringUntil('\r'); 
          };
           
          client.stop();
          
        }

        if(line != ""){
          
          sendOk = true;

          if(art == 0 || art == 1){
            char *s = new char[line.length() + 1];
            strcpy(s, line.c_str());
            std::string str(s);                                                       //char* to std::string

            if(str == "\n" || str == "Nok\n"){
              Serial.println("1");
              Serial.println(">>> Server Error (Nok)");
            } else {
              Serial.println("2");
              char* params = strtok(s, "&");
              params = strtok(NULL, "&");
              std::string zones(params);
              params = strtok(NULL, "&");   
              std::string onOff(params); 
              params = strtok(NULL, "&");   
              std::string zonesTimeIn(params);
              params = strtok(NULL, "&");   
              std::string priborTime(params);
              params = strtok(NULL, "&");   
              std::string priborZone(params);
              params = strtok(NULL, "&");   
              std::string activationIn(params);
    
              delete[] s;

              //-------------------Парсинг пришедших данных-----------------------
              char *m = new char[zones.size() + 1];
              strcpy(m, zones.c_str());
                
              vector<string> arr;
              string strr (zones);
              string delim("#");
              size_t prev = 0;
              size_t next;
              size_t delta = delim.length();
              
              while((next = strr.find(delim, prev)) != string::npos){
                string tmp = strr.substr(prev, next-prev);
                arr.push_back(strr.substr(prev, next-prev));
                prev = next + delta;
              }
              string tmp = strr.substr(prev);
              arr.push_back(strr.substr(prev));

              delete[] m;
                
              //-------------------Парсинг времени полива зон (длительность)-----------------------
              char *m1 = new char[zonesTimeIn.size() + 1];
              strcpy(m1, zonesTimeIn.c_str());
                
              vector<string> arr1;
              string str1 (zonesTimeIn);
              string delim1("#");
              size_t prev1 = 0;
              size_t next1;
              size_t delta1 = delim1.length();
              
              while((next1 = str1.find(delim1, prev1)) != string::npos){
                string tmp = str1.substr(prev1, next1-prev1);
                arr1.push_back(str1.substr(prev1, next1-prev1));
                prev1 = next1 + delta1;
              }
              string tmp1 = str1.substr(prev1);
              arr1.push_back(str1.substr(prev1));
              
              delete[] m1;

              char *m2 = new char[priborTime.size() + 1];
              strcpy(m2, priborTime.c_str());
                
              vector<string> arr2;
              string str2 (priborTime);
              string delim2("!");
              size_t prev2 = 0;
              size_t next2;
              size_t delta2 = delim2.length();
              
              while((next2 = str2.find(delim2, prev2)) != string::npos){
                string tmp = str2.substr(prev2, next2-prev2);
                arr2.push_back(str2.substr(prev2, next2-prev2));
                prev2 = next2 + delta2;
              }
              string tmp2 = str2.substr(prev2);
              arr2.push_back(str2.substr(prev2));
    
              delete[] m2;
                
              char *m3 = new char[priborZone.size() + 1];
              strcpy(m3, priborZone.c_str());
                
              vector<string> arr3;
              string str3 (priborZone);
              string delim3("#");
              size_t prev3 = 0;
              size_t next3;
              size_t delta3 = delim3.length();
              
              while((next3 = str3.find(delim3, prev3)) != string::npos){
                string tmp = str3.substr(prev3, next3-prev3);
                arr3.push_back(str3.substr(prev3, next3-prev3));
                prev3 = next3 + delta3;
              }
              string tmp3 = str3.substr(prev3);
              arr3.push_back(str3.substr(prev3));
                
              delete[] m3;
    
              char *m4 = new char[onOff.size() + 1];
              strcpy(m4, onOff.c_str());
                  
              vector<string> arr4;
              string str4 (onOff);
              string delim4("#");
              size_t prev4 = 0;
              size_t next4;
              size_t delta4 = delim4.length();
                
              while((next4 = str4.find(delim4, prev4)) != string::npos){
                string tmp = str4.substr(prev4, next4-prev4);
                arr4.push_back(str4.substr(prev4, next4-prev4));
                prev4 = next4 + delta4;
              }
              string tmp4 = str4.substr(prev4);
              arr4.push_back(str4.substr(prev4));
                
              delete[] m4;
  
              //-------------------Замена активации-----------------------

              art = atoi(::activationWifi.c_str());
              if(art != 3 && art != 4 && art != 5){
                 ::activationWifi = activationIn;
              }

              //----------------Сохранение всех параметров----------------
         
              if(art == 1){
                std::string kek[arr.size()];
                for(int i = 0; i < arr.size(); i++){
                  ::zoneName[i] = arr[i];
                }
                  
                std::string kek1[arr1.size()];
                for(int i = 0; i < arr1.size(); i++){
                  ::zoneTime[i] = arr1[i];
                }
                  
                std::string kek2[arr2.size()];
                for(int i = 0; i < arr2.size(); i++){
                  ::priborTimes[i] = arr2[i];
                }
                std::string zero = "";
                for(int i = arr2.size(); i < 350; i++){
                  ::priborTimes[i] = zero;
                }
  
                std::string kek3[arr3.size()];
                for(int i = 0; i < arr3.size(); i++){
                  ::priborZones[i] = arr3[i];
                }
                std::string zero1 = "";
                for(int i = arr3.size(); i < 350; i++){
                  ::priborZones[i] = zero1;
                }
  
                if(!zamenaNo){
                  std::string kek4[arr4.size()];
                  for(int i = 0; i < 10; i++){
                    ::zoneOnOff[i] = arr4[i];
                  }
                }
              }

              //-----------------Вывод на экран ----------------------------

              
                int len = sizeof(zoneName) / sizeof(zoneName[0]);
                for(int i = 0; i < len; i++){
                  Serial.print(zoneName[i].c_str());
                  Serial.print(" ");
                }
                Serial.println();
                  
                
                len = sizeof(zoneTime) / sizeof(zoneTime[0]);
                for(int i = 0; i < len; i++){
                  Serial.print(zoneTime[i].c_str());
                  Serial.print(" ");
                }
                Serial.println();

                len = sizeof(zoneOnOff) / sizeof(zoneOnOff[0]);
                for(int i = 0; i < len; i++){
                  Serial.print(zoneOnOff[i].c_str());
                  Serial.print(" ");
                }
                Serial.println();

                len = sizeof(priborTimes) / sizeof(priborTimes[0]);
                for(int i = 0; i < len; i++){
                  Serial.print(priborTimes[i].c_str());
                  Serial.print(" ");
                }
                Serial.println();

                len = sizeof(priborZones) / sizeof(priborZones[0]);
                for(int i = 0; i < len; i++){
                  Serial.print(priborZones[i].c_str());
                  Serial.print(" ");
                }
                Serial.println();
            
                Serial.print("Активация: ");Serial.println(::activationWifi.c_str());
          
                vTaskDelay(2000);

               //-----------------------------------------------------
  
              ::kolvoZaprosov++;
              Serial.print("Количество запросов: ");Serial.println(kolvoZaprosov);    
            }
          } else if(art == 2){
            
          } else {
            ::activationWifi = "0";
          }
        } 
      }
      xSemaphoreGive(xMutex);
      vTaskDelay(100);
     
    } else {
      Serial.println(nameWiFi);
      Serial.println(passwordWiFi);
      Serial.println(newEmail);
      Serial.println(email);

      int len = sizeof(zoneTime) / sizeof(zoneTime[0]);
      for(int i = 0; i < len; i++){
        Serial.print(zoneTime[i].c_str());
        Serial.print(" ");
      }
      Serial.println();

      vTaskDelay(2000);
	}
  }
}