# Barcode_Scanner
scene any barcode with pc camera
import cv2
from pyzbar.pyzbar import decode
import numpy as np

def scan_barcodes_live():
    # Initialize the video capture object (0 is usually the default camera)
    cap = cv2.VideoCapture(0)
    
    # Check if the camera opened successfully
    if not cap.isOpened():
        print("Error: Could not open video stream.")
        return

    print("Barcode scanner started. Press 'q' to exit.")

    while True:
        # Read frame from the camera
        ret, frame = cap.read()
        
        if not ret:
            print("Error: Could not read frame.")
            break

        # Decode all barcodes/QR codes in the frame
        decoded_objects = decode(frame)

        # Loop over the detected objects and draw a bounding box
        for obj in decoded_objects:
            # Print the decoded data and type
            barcode_data = obj.data.decode('utf-8')
            barcode_type = obj.type
            
            print(f"Detected: Type={barcode_type}, Data={barcode_data}")

            # Extract the bounding box location
            (x, y, w, h) = obj.rect
            
            # Draw a green rectangle around the barcode
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

            # Put the decoded text above the barcode
            text = f"{barcode_data} ({barcode_type})"
            cv2.putText(frame, text, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 2)

        # Display the frame
        cv2.imshow('Barcode and QR Code Scanner', frame)

        # Break the loop if 'q' is pressed
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    # Release the capture object and close all windows
    cap.release()
    cv2.destroyAllWindows()

if __name__ == '__main__':
    scan_barcodes_live()
