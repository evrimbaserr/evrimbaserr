import tkinter as tk
from tkinter import ttk, messagebox
import time

# Metni global olarak saklamak için değişken
metin_global = ""

# Stil ve tema ayarları
def stil_ayarla():
    style = ttk.Style()
    style.theme_use('clam')  # Modern bir tema seçiyoruz
    style.configure("TButton", font=("Verdana", 12, "bold"), padding=6, background="#4CAF50", foreground="white")
    style.map("TButton", background=[("active", "#45a049")], foreground=[("active", "white")])
    style.configure("TLabel", background="#e6e6fa", font=("Verdana", 12), padding=4)
    root.configure(bg="#e6e6fa")  # Arka plan rengini ayarladık.

# 1. Metinde verilen kelimelerin harflerini alt alta yazdırma
def harfleri_alt_alta_yazdir():
    sonuc = ""
    for kelime in metin_global.split():
        for harf in kelime:
            sonuc += harf + "\n"
        sonuc += "\n"
    messagebox.showinfo("Sonuç", sonuc)

# 2. Metnin orijinali, tamamen tersi ve kelime kelime tersi
def ters_islemleri():
    tamamen_tersi = metin_global[::-1]
    kelime_kelime_ters = ' '.join([kelime[::-1] for kelime in metin_global.split()])
    sonuc = f"Orijinal: {metin_global}\nTamamen Ters: {tamamen_tersi}\nKelime Kelime Ters: {kelime_kelime_ters}"
    messagebox.showinfo("Sonuç", sonuc)

# 3. Tüm "a" harflerini "A" yapma
def tum_a_harflerini_buyuk_yap():
    sonuc = metin_global.replace("a", "A").replace("â", "Â").replace("á", "Á")
    messagebox.showinfo("Sonuç", sonuc)

# 4. Kelimeleri alt alta yazdırma
def kelimeleri_alt_alta_yazdir():
    sonuc = "\n".join(metin_global.split())
    messagebox.showinfo("Sonuç", sonuc)

# 5. Ayrılan kelimeleri bitişik yazdırma
def kelimeleri_birlestir():
    global metin_global
    metin_global = ''.join(metin_global.split())
    messagebox.showinfo("Sonuç", f"Kelimelerin Bitişik Hali: {metin_global}")

# 6. Ünlü harf sayısını bulma
def unlu_harf_sayisi_bul():
    unlu_harfler = "aeıioöuüâá"
    sayi = sum(1 for harf in metin_global.lower() if harf in unlu_harfler)
    messagebox.showinfo("Sonuç", f"Ünlü harf sayısı: {sayi}")

# 7. Yazı yazma hızını hesaplama
def yazi_yazma_hizi_hesapla():
    window = tk.Toplevel(root)
    window.title("Yazı Yazma Hızı Hesaplama")
    window.configure(bg="#e6e6fa")

    ttk.Label(window, text="Metni buraya yazmaya başlamak için 'Başla'ya tıklayın").pack(pady=10)
    metin_entry = ttk.Entry(window, width=50, font=("Verdana", 12))
    metin_entry.pack()

    global baslangic_zamani

    def basla():
        global baslangic_zamani
        baslangic_zamani = time.time()
        messagebox.showinfo("Başla", "Yazmaya başlayabilirsiniz.")

    def bitti():
        bitis_zamani = time.time()
        metin = metin_entry.get()
        sure = bitis_zamani - baslangic_zamani
        hiz = len(metin) / sure if sure > 0 else 0
        sonuc = f"Yazma süresi: {sure:.2f} saniye\nSaniye başına {hiz:.2f} karakter."
        messagebox.showinfo("Sonuç", sonuc)

    ttk.Button(window, text="Başla", command=basla).pack(pady=5)
    ttk.Button(window, text="Bitti", command=bitti).pack(pady=5)

# Ana GUI
root = tk.Tk()
root.title("Metin Test Sayfası")
root.geometry("500x600")

stil_ayarla()  # Stil ayarını uyguluyoruz

# Metin girişi için alan
ttk.Label(root, text="Bir metin girin:").pack(pady=10)
metin_entry = ttk.Entry(root, width=50, font=("Verdana", 12))
metin_entry.pack(pady=5)

# Metni kaydet ve işlem menüsünü göster
def kaydet_metin():
    global metin_global
    metin_global = metin_entry.get()
    if metin_global:
        islemler_menusu()
    else:
        messagebox.showwarning("Uyarı", "Lütfen bir metin girin.")

# İşlem seçenekleri
def islemler_menusu():
    ttk.Button(root, text="Harfleri Alt Alta Yazdır", command=harfleri_alt_alta_yazdir).pack(pady=5)
    ttk.Button(root, text="Ters İşlemleri", command=ters_islemleri).pack(pady=5)
    ttk.Button(root, text="A Harflerini Büyüt", command=tum_a_harflerini_buyuk_yap).pack(pady=5)
    ttk.Button(root, text="Kelimeleri Alt Alta Yazdır", command=kelimeleri_alt_alta_yazdir).pack(pady=5)
    ttk.Button(root, text="Kelimeleri Bitişik Yazdır", command=kelimeleri_birlestir).pack(pady=5)
    ttk.Button(root, text="Ünlü Harf Sayısı", command=unlu_harf_sayisi_bul).pack(pady=5)
    ttk.Button(root, text="Yazı Yazma Hızı", command=yazi_yazma_hizi_hesapla).pack(pady=5)
    ttk.Button(root, text="ÇIKIŞ", command=root.quit).pack(pady=10)

# Metin kaydet düğmesi
ttk.Button(root, text="Metni Kaydet ve İşlemler Menüsüne Geç", command=kaydet_metin).pack(pady=20)

# Ana döngüyü başlat
root.mainloop()
