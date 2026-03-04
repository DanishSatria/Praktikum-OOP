# 🎮 Sistem Pertarungan Game RPG - Pembelajaran OOP Python

## 📋 Deskripsi Program

Program ini adalah implementasi lengkap pembelajaran **Pemrograman Berorientasi Objek (OOP)** menggunakan Python, dengan studi kasus **Sistem Pertarungan Game RPG**. Program ini mencakup semua konsep dasar hingga lanjutan OOP yang dipelajari di kelas XI SMK Telkom Malang.

Program ditulis dalam **SATU FILE** (`game_rpg.py`) yang berisi 6 latihan berurutan, masing-masing mendemonstrasikan konsep OOP yang berbeda.

---

## 📁 Kode Program Lengkap (`game_rpg.py`)

Latihan 1 & 2: Dasar Class dan Interaksi
Konsep: Membuat class Hero dengan constructor __init__

Atribut: name, hp, attack_power

Method: info(), serang(), diserang()

Analisis 1: Atribut publik bisa diubah langsung dari luar class

Analisis 2: Parameter lawan harus objek utuh untuk mengakses method diserang()

Latihan 3: Inheritance (Pewarisan)
Konsep: Class Mage mewarisi Hero dengan super()

Fitur baru: Atribut mana dan method skill_fireball()

Analisis 3: Tanpa super(), atribut induk tidak terinisialisasi → error

Latihan 4: Encapsulation (Enkapsulasi)
Konsep: Private attribute __hp dengan getter dan setter

Validasi: HP tidak boleh negatif atau > 1000

Analisis 4:

Name mangling (_HeroEncapsulated__hp) memungkinkan akses paksa

Setter penting untuk menjaga integritas data

Latihan 5: Abstraction & Interface
Konsep: Abstract class GameUnit dengan @abstractmethod

Implementasi: HeroUnit dan MonsterUnit wajib implementasi semua abstract method

Analisis 5:

Abstract class tidak bisa diinstansiasi

Berfungsi sebagai kontrak untuk class turunan

Latihan 6: Polymorphism
Konsep: Satu method serang() dengan implementasi berbeda di setiap class anak

Class anak: MagePolymorph, ArcherPolymorph, FighterPolymorph, HealerPolymorph

Analisis 6:

Polymorphism memudahkan penambahan class baru tanpa mengubah kode lama

Nama method harus konsisten
```python
"""
Sistem Pertarungan Game RPG - Pembelajaran OOP Python
SMK Telkom Malang - Fase F Kelas XI
File lengkap dengan 6 latihan dan semua jawaban analisis
"""

# ============================================
# LATIHAN 1 & 2: DASAR CLASS DAN INTERAKSI
# ============================================

class Hero:
    # Constructor: Dijalankan saat Hero baru dibuat
    def __init__(self, name, hp, attack_power):
        self.name = name          # Nama Hero
        self.hp = hp               # Nyawa (Health Point)
        self.attack_power = attack_power  # Kekuatan Serangan
    
    # Method untuk menampilkan info hero
    def info(self):
        print(f"Hero: {self.name} | HP: {self.hp} | Power: {self.attack_power}")
    
    # Method menyerang: Objek ini (self) menyerang objek lain (lawan)
    def serang(self, lawan):
        print(f"{self.name} menyerang {lawan.name}!")
        lawan.diserang(self.attack_power)
    
    # Method diserang: Menerima damage
    def diserang(self, damage):
        self.hp -= damage
        print(f"{self.name} terkena damage {damage}. Sisa HP: {self.hp}")

print("="*60)
print("LATIHAN 1 & 2: DASAR CLASS DAN INTERAKSI")
print("="*60)

# Membuat Object (Instansiasi)
hero1 = Hero("Layla", 100, 15)
hero2 = Hero("Zilong", 120, 20)

# Memanggil Method
hero1.info()
hero2.info()

# Tugas Analisis 1: Mengubah hp hero1
print("\n--- Tugas Analisis 1 ---")
hero1.hp = 500  # Mengubah hp menjadi 500
print(f"Setelah diubah: Hero: {hero1.name} | HP: {hero1.hp}")

print("\n--- Pertarungan Dimulai ---")
hero1.serang(hero2)
hero2.serang(hero1)


# ============================================
# LATIHAN 3: INHERITANCE (PEWARISAN)
# ============================================

print("\n" + "="*60)
print("LATIHAN 3: INHERITANCE (PEWARISAN)")
print("="*60)

# Class Mage adalah anak dari class Hero
class Mage(Hero):
    def __init__(self, name, hp, attack_power, mana):
        # Memanggil constructor milik Parent (Hero)
        super().__init__(name, hp, attack_power)
        self.mana = mana
    
    def info(self):
        print(f"{self.name} [Mage] | HP: {self.hp} | Mana: {self.mana} | Power: {self.attack_power}")
    
    # Mage punya skill khusus
    def skill_fireball(self, lawan):
        if self.mana >= 20:
            print(f"{self.name} menggunakan Fireball ke {lawan.name}!")
            self.mana -= 20
            lawan.diserang(self.attack_power * 2)  # Damage 2x lipat
        else:
            print(f"{self.name} gagal skill! Mana tidak cukup.")

print("\n--- Update Class Hero ---")
eudora = Mage("Eudora", 80, 30, 100)
balmond = Hero("Balmond", 200, 10)

eudora.info()
balmond.info()
print("\n--- Pertarungan Mage vs Hero ---")
eudora.serang(balmond)           # Serangan biasa (warisan dari Hero)
eudora.skill_fireball(balmond)   # Skill khusus Mage


# ============================================
# LATIHAN 4: ENCAPSULATION (ENKAPSULASI)
# ============================================

print("\n" + "="*60)
print("LATIHAN 4: ENKAPSULASI")
print("="*60)

class HeroEncapsulated:
    def __init__(self, nama, hp_awal):
        self.nama = nama
        # Enkapsulasi: HP bersifat Private
        self.__hp = hp_awal
    
    # GETTER: Cara resmi melihat HP
    def get_hp(self):
        return self.__hp
    
    # SETTER: Cara resmi mengubah HP (dengan validasi)
    def set_hp(self, nilai_baru):
        if nilai_baru < 0:
            print("Peringatan: HP tidak boleh negatif! HP di-set ke 0.")
            self.__hp = 0
        elif nilai_baru > 1000:
            print("Cheat terdeteksi! HP dimaksimalkan ke 1000 saja.")
            self.__hp = 1000
        else:
            self.__hp = nilai_baru
    
    def diserang(self, damage):
        # Kita pakai setter/getter agar aman
        sisa_hp = self.get_hp() - damage
        self.set_hp(sisa_hp)
        print(f"{self.nama} terkena damage {damage}. Sisa HP: {self.get_hp()}")

print("\n--- Uji Coba Enkapsulasi ---")
hero1_encap = HeroEncapsulated("Layla", 100)

# Mencoba akses langsung (akan error)
try:
    print(hero1_encap.__hp)
except AttributeError as e:
    print(f"Error akses langsung: {e}")

# Menggunakan setter dengan nilai negatif
print("\n1. Mencoba set HP negatif:")
hero1_encap.set_hp(-50)
print(f"HP setelah set -50: {hero1_encap.get_hp()}")

# Mencoba cheat
print("\n2. Mencoba cheat HP 9999:")
hero1_encap.set_hp(9999)
print(f"HP setelah cheat: {hero1_encap.get_hp()}")

# Tugas Analisis 4: Name Mangling
print("\n--- Tugas Analisis 4: Name Mangling ---")
print(f"Mencoba akses paksa: {hero1_encap._HeroEncapsulated__hp}")
print("(Ini adalah name mangling - Python mengubah __hp menjadi _NamaKelas__hp)")


# ============================================
# LATIHAN 5: ABSTRACTION & INTERFACE
# ============================================

print("\n" + "="*60)
print("LATIHAN 5: ABSTRACTION & INTERFACE")
print("="*60)

from abc import ABC, abstractmethod

# 1. Interface / Abstract Class
class GameUnit(ABC):
    @abstractmethod
    def serang(self, target):
        pass
    
    @abstractmethod
    def info(self):
        pass

# 2. Implementasi pada Class Konkret
class HeroUnit(GameUnit):
    def __init__(self, nama):
        self.nama = nama
    
    def serang(self, target):
        print(f"Hero {self.nama} menebas {target}!")
    
    def info(self):
        print(f"Saya adalah Hero: {self.nama}")

class MonsterUnit(GameUnit):
    def __init__(self, jenis):
        self.jenis = jenis
    
    def serang(self, target):
        print(f"Monster {self.jenis} menggigit {target}!")
    
    def info(self):
        print(f"Saya adalah Monster: {self.jenis}")

print("\n--- Uji Coba Abstract Class ---")
try:
    unit = GameUnit()
    print("Berhasil membuat objek GameUnit")
except TypeError as e:
    print(f"Error: {e}")
    print("(Abstract class tidak bisa diinstansiasi langsung)")

# Membuat objek dari class turunan
h = HeroUnit("Alucard")
m = MonsterUnit("Serigala")

print("\n--- Info Unit ---")
h.info()
m.info()

print("\n--- Serangan Unit ---")
h.serang("Zombie")
m.serang("Petualang")


# ============================================
# LATIHAN 6: POLYMORPHISM
# ============================================

print("\n" + "="*60)
print("LATIHAN 6: POLYMORPHISM")
print("="*60)

# Parent Class
class HeroPolymorph:
    def __init__(self, nama):
        self.nama = nama
    
    def serang(self):
        print("Hero menyerang dengan tangan kosong.")

# Child Class 1
class MagePolymorph(HeroPolymorph):
    def serang(self):
        print(f"{self.nama} (Mage) menembakkan Bola Api! Boom!")

# Child Class 2
class ArcherPolymorph(HeroPolymorph):
    def serang(self):
        print(f"{self.nama} (Archer) memanah dari jauh! Jleb!")

# Child Class 3
class FighterPolymorph(HeroPolymorph):
    def serang(self):
        print(f"{self.nama} (Fighter) memukul dengan pedang! Slash!")

# Child Class 4 (Tugas Analisis 6)
class HealerPolymorph(HeroPolymorph):
    def serang(self):
        print(f"{self.nama} (Healer) tidak menyerang, tapi menyembuhkan teman!")

print("\n--- PERANG DIMULAI ---")
# Kita punya daftar hero campuran
pasukan = [
    MagePolymorph("Eudora"),
    ArcherPolymorph("Miya"),
    FighterPolymorph("Zilong"),
    MagePolymorph("Gord"),
    HealerPolymorph("Estes")
]

# Satu perintah loop, tapi respon berbeda-beda (Polymorphism)
for pahlawan in pasukan:
    pahlawan.serang()

print("\n" + "="*60)
print("KESIMPULAN PEMBELAJARAN OOP")
print("="*60)
print("""
1. Class & Object: Blueprint dan hasil cetakannya
2. Inheritance: Mage mewarisi sifat Hero
3. Encapsulation: Melindungi data HP dengan private attribute
4. Abstraction: Membuat kontrak dengan abstract class
5. Polymorphism: Method sama, perilaku berbeda
""")


