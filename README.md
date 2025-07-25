# tourism-analysis-guanajuato
# tourism-guanajuato.ipynb

# 1. Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 2. Load dataset
data = pd.read_csv('data/tourism_guanajuato.csv')

# 3. Data overview
print("Dataset shape:", data.shape)
print(data.head())

# 4. Convert Month to categorical with order
months_order = ["January", "February", "March", "April", "May", "June", 
                "July", "August", "September", "October", "November", "December"]
data['Month'] = pd.Categorical(data['Month'], categories=months_order, ordered=True)

# 5. Total visitors by year and month
visitors_month = data.groupby(['Year', 'Month'])['Visitors'].sum().reset_index()

plt.figure(figsize=(12,6))
sns.lineplot(data=visitors_month, x='Month', y='Visitors', hue='Year', marker='o')
plt.title('Total Visitors per Month by Year')
plt.ylabel('Number of Visitors')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('visuals/visitors_per_month.png')
plt.show()

# 6. Visitors by city
visitors_city = data.groupby('City')['Visitors'].sum().sort_values(ascending=False)

plt.figure(figsize=(8,5))
sns.barplot(x=visitors_city.values, y=visitors_city.index, palette='viridis')
plt.title('Total Visitors by City')
plt.xlabel('Visitors')
plt.ylabel('City')
plt.tight_layout()
plt.savefig('visuals/visitors_by_city.png')
plt.show()

# 7. Domestic vs International visitors
visitors_type = data.groupby('Tourist_Type')['Visitors'].sum().reset_index()

plt.figure(figsize=(6,6))
plt.pie(visitors_type['Visitors'], labels=visitors_type['Tourist_Type'], autopct='%1.1f%%', colors=['#66b3ff','#ff9999'])
plt.title('Domestic vs International Visitors')
plt.savefig('visuals/domestic_vs_international.png')
plt.show()

# 8. Average spending comparison
avg_spending_type = data.groupby('Tourist_Type')['Avg_Spending'].mean().reset_index()

plt.figure(figsize=(6,4))
sns.barplot(data=avg_spending_type, x='Tourist_Type', y='Avg_Spending', palette='pastel')
plt.title('Average Spending by Tourist Type (MXN)')
plt.ylabel('Average Spending (MXN)')
plt.tight_layout()
plt.savefig('visuals/avg_spending_by_type.png')
plt.show()

# 9. Event attendance over months (only some months available, so group by month)
event_attendance_month = data.groupby('Month')['Event_Attendance'].sum().reset_index()

plt.figure(figsize=(12,6))
sns.barplot(data=event_attendance_month, x='Month', y='Event_Attendance', palette='magma')
plt.title('Event Attendance by Month')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('visuals/event_attendance_by_month.png')
plt.show()

# 10. Insights summary
print("""
Insights:
- Guanajuato City and San Miguel de Allende lead in visitor numbers.
- October has the highest number of visitors due to festivals like Cervantino.
- Domestic visitors are more numerous, but international tourists spend more on average.
- Event attendance peaks in October, showing strong cultural engagement.
""")
