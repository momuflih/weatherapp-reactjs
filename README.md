# weatherapp-reactjs
COMPANY: CODITECH SOLUTIONS
NAME: MOHAMED MUFLIH
INTERN ID:CT08NLU
DOMAIN:REACT
BATCH DURATION:january 15th/2025 to february 15th/2025
This code demonstrates a simple React-based weather application that fetches weather data from an external API (OpenWeatherMap API) based on user input. Let’s break down the structure and functionality of the application step-by-step.

### 1. **Setting Up State Variables**:
In the React component `App`, three state variables are initialized using the `useState` hook:
- `city`: This state holds the name of the city entered by the user. It is updated whenever the user types in the input field.
- `weatherData`: This state stores the fetched weather data, which includes temperature, humidity, wind speed, and weather description.
- `error`: This state is used to store error messages, such as when the city entered by the user cannot be found.

```js
const [city, setCity] = useState('');
const [weatherData, setWeatherData] = useState(null);
const [error, setError] = useState(null);
```

### 2. **Handling Input**:
The app includes an input field for users to type in the name of the city. The `handleInputChange` function is triggered whenever the user types into the input field. It updates the `city` state, which ensures that the latest city name entered by the user is stored.

```js
const handleInputChange = (e) => {
  setCity(e.target.value);
};
```

### 3. **Fetching Weather Data**:
When the user clicks the "Get Weather" button, the `fetchWeatherData` function is called. This function performs the actual fetching of weather data from the OpenWeatherMap API using Axios, a popular library for making HTTP requests.

```js
const fetchWeatherData = async () => {
  if (!city) return;  // Exit if no city is entered

  setError(null);  // Reset previous error message
  setWeatherData(null);  // Reset previous weather data

  try {
    const response = await axios.get(API_URL, {
      params: {
        q: city,  // The city entered by the user
        appid: API_KEY,  // Your OpenWeatherMap API key
        units: 'metric',  // Metric units for temperature (Celsius)
      },
    });
    setWeatherData(response.data);  // Store the fetched data
  } catch (err) {
    setError('City not found, please try again.');  // Handle errors (e.g., invalid city)
  }
};
```

- **API URL and Parameters**:
  The `axios.get` method sends a GET request to the OpenWeatherMap API with the city name (`q: city`), the API key (`appid: API_KEY`), and the temperature unit set to Celsius (`units: 'metric'`).
  
- **Error Handling**:
  If there’s an issue with the request (e.g., city not found or network issues), the `catch` block captures the error and sets an error message in the `error` state.

### 4. **Displaying Data**:
Once the data is successfully fetched, it is stored in the `weatherData` state. The app conditionally renders the weather information on the screen. The `weatherData` object contains multiple properties such as `name`, `sys.country`, `main.temp`, `main.humidity`, `weather[0].description`, and `wind.speed`.

```js
{weatherData && (
  <div className="weather-info">
    <h2>{weatherData.name}, {weatherData.sys.country}</h2>
    <p><strong>Temperature:</strong> {weatherData.main.temp}°C</p>
    <p><strong>Humidity:</strong> {weatherData.main.humidity}%</p>
    <p><strong>Weather:</strong> {weatherData.weather[0].description}</p>
    <p><strong>Wind Speed:</strong> {weatherData.wind.speed} m/s</p>
  </div>
)}
```

- **Conditionally Rendered Weather Data**: If `weatherData` is not `null`, it displays the weather details such as:
  - **City Name and Country**: Retrieved from `weatherData.name` and `weatherData.sys.country`.
  - **Temperature**: In Celsius from `weatherData.main.temp`.
  - **Humidity**: From `weatherData.main.humidity`.
  - **Weather Description**: A text description of the weather (e.g., "clear sky") from `weatherData.weather[0].description`.
  - **Wind Speed**: From `weatherData.wind.speed`.

### 5. **Error Handling**:
If the user enters a city that doesn’t exist or there’s another error, the app will display an error message stored in the `error` state. This message is rendered inside a `<p>` tag with the class `error-message`.

```js
{error && <p className="error-message">{error}</p>}
```

### 6. **Styling**:
The application includes basic CSS in `App.css` to style the UI elements such as the input field, buttons, and weather data display. The CSS defines layout properties for centering the content, styling the input and button elements, and adding margins and padding for a better user experience.

```css
.input-container {
  margin-bottom: 20px;
}

input {
  padding: 10px;
  font-size: 1rem;
  width: 200px;
  margin-right: 10px;
}

button {
  padding: 10px;
  font-size: 1rem;
  cursor: pointer;
}

.weather-info {
  margin-top: 20px;
}

.weather-info p {
  font-size: 1.2rem;
}

.error-message {
  color: red;
  font-size: 1.2rem;
}
```

### 7. **API Key**:
The app requires an API key from OpenWeatherMap, which must be inserted in the `API_KEY` constant for the app to work. You can get a free API key by signing up at [OpenWeatherMap](https://openweathermap.org/api).

### Conclusion:
This weather app allows users to input a city name, fetch current weather data, and display relevant details like temperature, humidity, weather description, and wind speed. It handles errors gracefully and provides a simple yet functional user interface. This app serves as a great foundation that can be expanded with additional features, such as displaying forecasts, adding more styling, or including additional weather parameters like pressure or UV index.
