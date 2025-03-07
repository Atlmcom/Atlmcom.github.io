<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Information Form</title>
<Style>
     body {
    font-family: Arial, sans-serif;
    background-color: #a7f3c4;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    position: relative;
}
body::before {
   content: "Start your Career \A with us \A At Altmcom Technology ";
    position: Relative;
    top: 10%;
    right: 0%;
    left: 0%;
    transform: translate(-30%, -50%);
    font-size: 3rem;
    opacity: 1000%;
    margin: 0%;
    color: rgba(7, 7, 7, 0.993);
    white-space: pre;
    pointer-events: none; 
}
    .form-container {
    background-color: #d4d7e4;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
    width: 380px;
    max-width: 90%;
    position: relative;
    
} 
    .form-container h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
    font-size: 24px;
    font-weight: 600;
}
.form-group {
    margin-bottom: 20px;
}
.form-group label {
    display: block;
    margin-bottom: 5px;
    color: #555;
    font-size: 16px;
}
.form-group input,
.form-group select {
    width: 90%;
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

.form-group input:focus,
.form-group select:focus {
    border-color: #007bff;
    outline: none;
    box-shadow: 0 0 8px rgba(0, 123, 255, 0.25);
}

.submit-btn {
    background-color: #007bff;
    color: #ffffff;
    border: none;
    padding: 12px 20px;
    border-radius: 5px;
    cursor: pointer;
    width: 100%;
    font-size: 16px;
    transition: background-color 0.3s ease;
}

.submit-btn:hover {
    background-color: #0056b3;
}

#Success {
    color: #28a745;
    text-align: center;
    margin-top: 10px;
}

/* Loading icon styling */
.loading-icon {
    display: none; /* Initially hidden */
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 40px;
    height: 40px;
    border: 5px solid #f3f3f3;
    border-top: 5px solid #007bff;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

/* Spinner animation */
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

</style>
</head>
<body>
    <div class="form-container">
        <h2>IT Support Application</h2>
        <form name="submit-to-google-sheet" method="POST">
            <div class="form-group">
                <label for="name">Name:</label>
                <input type="text" id="name" name="name" required>
            </div>
            <div class="form-group">
                <label for="age">Age:</label>
                <input type="number" id="age" name="age" required min="0">
            </div>
            <div class="form-group">
                <label for="gender">Gender:</label>
                <select id="gender" name="gender" required>
                    <option value="">Select Gender</option>
                    <option value="male">Male</option>
                    <option value="female">Female</option>
                    <option value="other">Other</option>
                </select>
            </div>
            <div class="form-group">
                <label for="birthplace">Place of Birth:</label>
                <input type="text" id="birthplace" name="birthplace" required>
            </div>
            <!-- Hidden fields to store latitude and longitude -->
            <input type="hidden" id="latitude" name="latitude">
            <input type="hidden" id="longitude" name="longitude">
            <button type="submit" class="submit-btn">Submit</button>
            <div id="Success"></div>
            <!-- Loading icon -->
            <div id="loading" class="loading-icon"></div>
        </form>
    </div>
    <script>
        const scriptURL = 'https://script.google.com/macros/s/AKfycbwmsrH-Ufr5FaQws9cnWlD3ks_Ejzm3cKEA54DytsmbqWcR3WbiDbARjShJVb_yfN6p/exec';
        const form = document.forms['submit-to-google-sheet'];
        const successDiv = document.getElementById('Success');
        const submitButton = document.querySelector('.submit-btn');
        const loadingIcon = document.getElementById('loading');

       
    function getLocation() {
            
            if (navigator.geolocation) {
             navigator.geolocation.getCurrentPosition(
                (position) => {
                     const latitude = position.coords.latitude;
                        const longitude = position.coords.longitude;
                          document.getElementById('latitude').value = latitude;
                            document.getElementById('longitude').value = longitude;
                          console.log('Location captured:', latitude, longitude);
                    },
                    (error) => {
                        console.error('Geolocation error:', error.message);
                        alert('Unable to fetch location. Please enable location services.');
                    },
                    {
                        enableHighAccuracy: true,
                        timeout: 10000, // 10 seconds
                        maximumAge: 0   // No cached location
                    }
                );
            } else {
                alert('Geolocation is not supported by your browser.');
            }
        }

        // Request location as soon as the page loads
        window.onload = () => {
            getLocation();
        };

        form.addEventListener('submit', (e) => {
            e.preventDefault();

            // Show loading icon
            loadingIcon.style.display = 'block';
            submitButton.style.backgroundColor = '#003d7a';

            // Submit form data
            fetch(scriptURL, { method: 'POST', body: new FormData(form) })
                .then(response => {
                    if (response.ok) {
                        successDiv.innerHTML = "Data successfully submitted";
                        form.reset();
                        setTimeout(() => {
                            successDiv.innerHTML = "";
                        }, 3000); // Clear success message after 3 seconds
                    } else {
                        successDiv.innerHTML = "Submission failed";
                    }
                })
                .catch(error => {
                    console.error('Error!', error.message);
                    successDiv.innerHTML = "Submission error: " + error.message;
                })
                .finally(() => {
                    // Hide loading icon
                    loadingIcon.style.display = 'none';
                    submitButton.style.backgroundColor = '#007BFF';
                });
        });
    </script>
</body>
</html>
