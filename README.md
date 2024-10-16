# Application-of-Python-in-the-field-of-Concrete-Technology
ASSIGNMENT NO. 4

Q.1) # Input for characteristic compressive strength and other parameters
fck = float(input("Enter the value of characteristic compressive strength (MPa): "))

# Experimental Determinations
Gca = float(input("Enter the value of specific gravity of CA: "))
Gfa = float(input("Enter the value of specific gravity of FA: "))
Gc = float(input("Enter the value of specific gravity of cement: "))
Water_Density = float(input("Enter the value of water density (kg/m³): "))
AGG_Size = float(input("Enter the nominal size of aggregate (mm): "))
Nature_of_AGG = input("Nature of Aggregates: ")
Slump = float(input("Enter the value of workability of concrete (mm): "))
Admixture = input("Type of Admixture: ")
Exposure_Condition = input("Exposure Condition: ")
Concreting = input("Type of Concreting: ")
Zone = int(input("Zone (1-4): "))

# Target Mean Strength
sigma = {10: 3.5, 15: 3.5, 20: 4, 30: 5, 35: 5, 40: 5, 45: 5, 50: 5, 55: 5}
ft = fck + sigma.get(fck, 0) * 1.65
print("Target Mean Strength:", ft, "MPa")

# Maximum Free Water-Cement Ratio
# Reference IS:456-2000, Table 5
if Concreting == "Plain":
    WC_ratio = {"Mild": 0.6, "Moderate": 0.6, "Severe": 0.5, "Very Severe": 0.45, "Extreme": 0.4}
else:
    WC_ratio = {"Mild": 0.55, "Moderate": 0.5, "Severe": 0.45, "Very Severe": 0.45, "Extreme": 0.4}
    
WC_ratio_value = WC_ratio.get(Exposure_Condition, 0)
print("W/C Ratio:", WC_ratio_value)

# Minimum Cement Content
if Concreting == "Plain":
    Min_Cement_Content = {"Mild": 220, "Moderate": 240, "Severe": 250, "Very Severe": 260, "Extreme": 280}
else:
    Min_Cement_Content = {"Mild": 300, "Moderate": 300, "Severe": 320, "Very Severe": 340, "Extreme": 360}

Min_Cement_Value = Min_Cement_Content.get(Exposure_Condition, 0)
print("Minimum Cement Content:", Min_Cement_Value, "kg/m³")

# Water Content
water_content = {10: 208, 20: 186, 40: 165}
Water_Content = water_content.get(AGG_Size, 0)

if Slump == 75:
    Water_Content += Water_Content * 0.03
elif Slump == 100:
    Water_Content += Water_Content * 0.06
elif Slump == 125:
    Water_Content += Water_Content * 0.12
elif Slump == 175:
    Water_Content += Water_Content * 0.15
elif Slump == 200:
    Water_Content += Water_Content * 0.18

if Nature_of_AGG == "Sub-Angular":
    Water_Content -= 10
elif Nature_of_AGG == "Gravel":
    Water_Content -= 20
elif Nature_of_AGG == "Round":
    Water_Content -= 25

if Admixture == "Plasticiser":
    Water_Content -= 0.1 * Water_Content
elif Admixture == "Super-Plasticiser":
    Water_Content -= 0.2 * Water_Content

print("Water Content:", Water_Content, "kg/m³")

# Cement Content
Cement_Content = Water_Content / WC_ratio_value
print("Cement Content:", Cement_Content, "kg/m³")
print("As per IS:456-2000, Maximum allowed Cement Content is 450 kg/m³")

if Cement_Content > 450:
    Cement_Content = 450

if Cement_Content < 450:
    print("SAFE")

# Volume Calculations
Vol_Cement = Cement_Content / (Gc * Water_Density)
print("Volume of cement:", Vol_Cement, "m³")

Vol_Water = Water_Content / Water_Density
print("Volume of water:", Vol_Water, "m³")

Vol_AGG = 1 - Vol_Water - Vol_Cement
print("Volume of Course Aggregates and Fine Aggregates:", Vol_AGG, "m³")

# Zone ID
Zone_ID = {
    1: {10: 0.44, 20: 0.60, 40: 0.69},
    2: {10: 0.46, 20: 0.62, 40: 0.71},
    3: {10: 0.48, 20: 0.64, 40: 0.73},
    4: {10: 0.50, 20: 0.66, 40: 0.75}
}

Fraction = Zone_ID[Zone].get(AGG_Size, 0)
if WC_ratio_value == 0.5:
    fraction = Fraction
elif WC_ratio_value == 0.45:
    fraction = Fraction + (0.01 * Fraction)
elif WC_ratio_value == 0.4:
    fraction = Fraction + (0.02 * Fraction)
elif WC_ratio_value == 0.55:
    fraction = Fraction - (0.01 * Fraction)
elif WC_ratio_value == 0.6:
    fraction = Fraction - (0.02 * Fraction)

print("Course Aggregate Fraction:", fraction)

Vol_CA = Vol_AGG * fraction
print("Volume of Course Aggregate:", Vol_CA, "m³")

Vol_FA = Vol_AGG - Vol_CA
print("Volume of Fine Aggregate:", Vol_FA, "m³")

Mass_CA = Vol_CA * Gca * Water_Density
print("Mass of Course Aggregates:", Mass_CA, "kg/m³")

Mass_FA = Vol_FA * Gfa * Water_Density
print("Mass of Fine Aggregates:", Mass_FA, "kg/m³")

# Ratios
print("Weight Batching")
print(Cement_Content / Cement_Content, ";", Mass_FA / Cement_Content, ";", Mass_CA / Cement_Content, ";", Water_Content / Cement_Content)

print("Volume Batching")
print(Vol_Cement / Vol_Cement, ";", Vol_FA / Vol_Cement, ";", Vol_CA / Vol_Cement, ";", Vol_Water / Vol_Cement, ";")

OUTPUT:
Enter the value of characteristic compressive strength (MPa): 40
Enter the value of specific gravity of CA: 2.74
Enter the value of specific gravity of FA: 2.74
Enter the value of specific gravity of cement: 3.15
Enter the value of water density (kg/m³): 1000
Enter the nominal size of aggregate (mm): 20
Nature of Aggregates: Sub Angular
Enter the value of workability of concrete (mm): 100
Type of Admixture: Super Plasticiser
Exposure Condition: Severe
Type of Concreting: Reinforced
Zone (1-4): 1
Target Mean Strength: 48.25 MPa
W/C Ratio: 0.45
Minimum Cement Content: 320 kg/m³
Water Content: 197.16 kg/m³
Cement Content: 438.1333333333333 kg/m³
As per IS:456-2000, Maximum allowed Cement Content is 450 kg/m³
Volume of cement: 0.1390899470899471 m³
Volume of water: 0.19716 m³
Volume of Course Aggregates and Fine Aggregates: 0.6637500529100528 m³
Course Aggregate Fraction: 0.606
Volume of Course Aggregate: 0.402232532063492 m³
Volume of Fine Aggregate: 0.2615175208465608 m³
Mass of Course Aggregates: 1102.1171378539682 kg/m³
Mass of Fine Aggregates: 716.5580071195767 kg/m³
Weight Batching
1.0 ; 1.6354793223970863 ; 2.51548342480364 ; 0.45
Volume Batching
1.0 ; 1.8802043304930003 ; 2.891887878880097 ; 1.4175
