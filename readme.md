# Accessing User Data Flow Documentation

## Introduction

This document outlines the step-by-step process for Authorized Personnel to access user data in the AI-based Unique Identification Card System (UICS), with a maximum of three attempts to match the face and fingerprint.

## Flow Overview

1. **User Authentication:**
   - **Step 1:** First, Authorized Personnel log in to their portal using their unique username and password credentials.

2. **QR Code Scanning:**
   - **Step 2:** After logging in, Authorized Personnel can scan a QR code from a user's UICS.
   - **Step 3:** The scanned QR code provides basic user data in JSON format, including the user's name and their Unique 16-Digit Card Number.
   - ``` Json
        {
            "card_number": "1234-5678-9012-3456", // 16-digit card number,
            "name":"Vaibhav Singh Tanwar"
            
        }
        // Data Which Is Basically Stored In QrCode
     ```

3. **Face Recognition:**
   - **Step 4:** Authorized Personnel initiate face recognition by scanning the face of the user.
   - **Step 5:** A request is sent to the server, including the user's base64-encoded facial image and the user's 16-digit card number.
     -    <details>
          <summary><i>More Info</i></summary>
          Authorized Personnel initiate face recognition by scanning the user's facial image, which is then sent as a base64-encoded data along with the user's 16-digit card number to the server. The server performs facial recognition by comparing the scanned image with the stored reference data, determining whether there is a match. A successful match grants access, while an unsuccessful match results in denial. This step enhances security by ensuring that the user's facial features are accurately verified before proceeding with the authentication process.
          </details>
    - ``` Json
        {
            "card_number": "1234-5678-9012-3456", // 16-digit card number,
            "face_image": "base64-encoded-image-data", // Base64-encoded
        }
        // Data Which Is Send to Server For Verification
      ```
   - **Decision 2:** If the server's response indicates that the facial recognition was unsuccessful and the maximum number of attempts (3) has been reached, access to the user data is denied, and the process terminates.
   -  ```json
            {
                "is_matched":false, //bool field
            }
            //Response Recived From The Serevr
        ```
   - **Step 6:** If the facial recognition is unsuccessful but fewer than three attempts have been made, repeat Step 4 to make additional attempts.
   - **Step 7:** If successful, proceed to the next step.
      -  ```json
            {
                "is_matched":true, //bool field
                "token1":""
            }
            //Response Recived From The Serevr
         ```


4. **Fingerprint Authentication:**
   - **Step 8:** Authorized Personnel proceed to scan the user's fingerprint.
   - **Step 9:** Another request is sent to the server, including the user's fingerprint data and the 16-digit card number.
      -   <details>
          <summary><i>Upon receiving the request containing the user fingerprint data and the 16-digit card number, the backend initiates the fingerprint matching process and will response of matching status.</i></summary>
          <b>upon receiving the fingerprint authentication request in the backend, the server begins by parsing the request to extract the user's fingerprint data and the associated 16-digit card number. It then identifies the user using the card number and retrieves the stored reference fingerprint data for that user. A specialized fingerprint matching algorithm is applied to compare the provided fingerprint data with the stored reference data, with consideration for a predefined matching threshold. If the provided fingerprint closely matches the reference data and meets or exceeds the threshold, a positive authentication response is generated. Otherwise, a negative response is generated, indicating a failed authentication attempt. The outcome is logged, and if successful, the user's data is retrieved for further processing.</b>
          </details>

   - ```json
        {
            "card_number": "1234-5678-9012-3456", // 16-digit card number
            "fingerprint_data": "base64-encoded-fingerprint-data", // Base64-encoded fingerprint data
        }
        //Data Which Is Send to Server For Verification of Fingerprint

     ```
   - **Decision 3:** If the server's response indicates that the fingerprint authentication was unsuccessful and the maximum number of attempts (3) has been reached, access to the user data is denied, and the process terminates.
   - **Step 10:** If the fingerprint authentication is unsuccessful but fewer than three attempts have been made, repeat Step 8 to make additional attempts.
   - **Step 11:** If successful, proceed to the next step.
   -  ```json
            {
                "is_matched":true, //bool field
                "token2":""
            }
            //Response Recived From The Serevr
        ```

5. **User Data Retrieval:**
   - **Step 12:** If both facial recognition and fingerprint authentication are successful, Authorized Personnel receive the requested user data in JSON format.
   -  ```json
            {
                "token1":"",
                "token2":""
            }
            //Data Which Is Send to Server For Retrival Of Userdata

        ```
   - If BBoth Token Are Valid Authorized Personnel Will Recive The User Data From Backend
   -  ```json
            {
                "user_name": "Vaibhav Singh Tanwar",
                "age": 21,
                "address": "Chandigarh University (CU) in Mohali, 140413 Punjab, India.",
                // Additional user data fields
            }
            //
        ```
## Conclusion

This document outlines the secure and step-by-step process for Authorized Personnel to access user data using the AI-based Unique Identification Card System (UICS), incorporating a maximum of three attempts for both facial recognition and fingerprint authentication. Access is granted only if the required authentication steps are successfully completed within the specified limit.

![flowchart.png](flowchart.png)
