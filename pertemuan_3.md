# 1. Baca dataset 
df = pd.read_csv("/content/sample_data/TGM 2020-2023-eng.csv", sep=";", decimal=",")

# 2. Lihat ringkasan data 
print(df.info())
print(df.describe())

# --- Fungsi untuk deteksi & hapus outlier dengan IQR 
def remove_outliers_iqr(data, column):
    Q1 = data[column].quantile(0.25)
    Q3 = data[column].quantile(0.75)
    IQR = Q3 - Q1

    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    cleaned_data = data[(data[column] >= lower_bound) & (data[column] <= upper_bound)]
    return cleaned_data, lower_bound, upper_bound

# 3. Contoh: cek outlier pada kolom Daily Reading Duration
col = "Daily Reading Duration (in minutes)"

plt.figure(figsize=(12,5))

# Histogram sebelum
plt.subplot(1,2,1)
plt.hist(df[col], bins=20, color="skyblue", edgecolor="black")
plt.title(f"Before Outlier Removal: {col}")

# Hapus outlier
df_cleaned, low, up = remove_outliers_iqr(df, col)

# Histogram sesudah
plt.subplot(1,2,2)
plt.hist(df_cleaned[col], bins=20, color="lightgreen", edgecolor="black")
plt.title(f"After Outlier Removal: {col}")

plt.tight_layout()
plt.show()

print(f"Range batas IQR untuk {col}: {low:.2f} – {up:.2f}")
print(f"Jumlah data awal: {len(df)}, setelah bersih: {len(df_cleaned)}")
