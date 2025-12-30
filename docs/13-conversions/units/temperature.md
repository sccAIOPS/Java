# Temperature Conversion

> **Category:** Conversions  
> **Subcategory:** Unit Conversions  
> **Implementation:** [`TemperatureConverter.java`](../../src/main/java/com/thealgorithms/conversions/TemperatureConverter.java)

---

## 📚 Overview

Temperature conversion algorithms transform temperature values between different scales: Celsius, Fahrenheit, and Kelvin. These conversions are essential in scientific computing, weather applications, and international software.

**Key Characteristics:**
- Linear transformations with offsets
- Kelvin is the SI base unit
- Negative absolute zero is impossible in Kelvin

---

## 🔢 Mathematical Foundation

### Conversion Formulas

| From | To | Formula |
|------|-----|---------|
| Celsius | Fahrenheit | $F = C \times \frac{9}{5} + 32$ |
| Fahrenheit | Celsius | $C = (F - 32) \times \frac{5}{9}$ |
| Celsius | Kelvin | $K = C + 273.15$ |
| Kelvin | Celsius | $C = K - 273.15$ |
| Fahrenheit | Kelvin | $K = (F - 32) \times \frac{5}{9} + 273.15$ |
| Kelvin | Fahrenheit | $F = (K - 273.15) \times \frac{9}{5} + 32$ |

### Key Reference Points

| Point | Celsius | Fahrenheit | Kelvin |
|-------|---------|------------|--------|
| Absolute Zero | -273.15°C | -459.67°F | 0 K |
| Water Freezing | 0°C | 32°F | 273.15 K |
| Water Boiling | 100°C | 212°F | 373.15 K |

---

## 📊 Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Any conversion | O(1) | O(1) |

---

## 🔄 Algorithm (Pseudocode)

```
ALGORITHM CelsiusToFahrenheit(celsius)
─────────────────────────────────────────────────────
    RETURN celsius × (9/5) + 32

ALGORITHM FahrenheitToCelsius(fahrenheit)
─────────────────────────────────────────────────────
    RETURN (fahrenheit - 32) × (5/9)

ALGORITHM CelsiusToKelvin(celsius)
─────────────────────────────────────────────────────
    RETURN celsius + 273.15

ALGORITHM KelvinToCelsius(kelvin)
─────────────────────────────────────────────────────
    IF kelvin < 0 THEN
        ERROR "Invalid: below absolute zero"
    END IF
    RETURN kelvin - 273.15
```

---

## 💻 Implementation Notes

### Java Implementation

```java
public class TemperatureConverter {
    
    public static double celsiusToFahrenheit(double celsius) {
        return celsius * 9.0 / 5.0 + 32;
    }
    
    public static double fahrenheitToCelsius(double fahrenheit) {
        return (fahrenheit - 32) * 5.0 / 9.0;
    }
    
    public static double celsiusToKelvin(double celsius) {
        return celsius + 273.15;
    }
    
    public static double kelvinToCelsius(double kelvin) {
        if (kelvin < 0) {
            throw new IllegalArgumentException(
                "Temperature below absolute zero");
        }
        return kelvin - 273.15;
    }
}
```

### Code Reference

📁 **Source File:** [`src/main/java/com/thealgorithms/conversions/TemperatureConverter.java`](../../src/main/java/com/thealgorithms/conversions/TemperatureConverter.java)

---

## 🌍 Real-World Applications

### 1. Weather Applications
**Use Case:** Display temperature in user's preferred scale

### 2. Scientific Computing
**Use Case:** Converting between laboratory measurements

### 3. HVAC Systems
**Use Case:** Thermostat and climate control

### 4. International Software
**Use Case:** Localized temperature display

---

## ⚠️ Common Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Integer division | 5/9 = 0 in integer math | Use 5.0/9.0 |
| Negative Kelvin | Physically impossible | Validate input |
| Precision loss | Floating point errors | Use BigDecimal for precision |

---

## 📖 References

1. **NIST Reference** - Temperature scales
2. **SI Unit Definitions** - Kelvin

---

## 🔗 Related Algorithms

- [Unit Conversions](./unit-conversions.md)
- [Affine Converter](./affine-converter.md)

---

*Last updated: December 30, 2025*
