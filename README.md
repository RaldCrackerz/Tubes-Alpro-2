# Tubes-Alpro-2
package main

import (
    "fmt"
)

// Struktur data untuk tim e-sports
type Team struct {
    Name      string
    Wins      int
    ScoreDiff int
}

// Struktur data untuk pertandingan
type Match struct {
    TeamA  string
    TeamB  string
    ScoreA int
    ScoreB int
}

const maxTeams = 100
const maxMatches = 100

var teams [maxTeams]Team
var teamCount int

var matches [maxMatches]Match
var matchCount int

// Tambah tim baru
func addTeam(name string) {
    if teamCount < maxTeams {
        teams[teamCount] = Team{Name: name}
        teamCount++
        fmt.Println("Tim berhasil ditambahkan.")
    } else {
        fmt.Println("Kapasitas tim penuh.")
    }
}

// Perbarui nama tim berdasarkan indeks
func updateTeam(idx int, newName string) {
    if idx >= 0 && idx < teamCount {
        teams[idx].Name = newName
        fmt.Println("Nama tim berhasil diperbarui.")
    } else {
        fmt.Println("Indeks tim tidak valid.")
    }
}

// Hapus tim berdasarkan indeks (geser elemen)
func deleteTeam(idx int) {
    if idx >= 0 && idx < teamCount {
        for i := idx; i < teamCount-1; i++ {
            teams[i] = teams[i+1]
        }
        teamCount--
        fmt.Println("Tim berhasil dihapus.")
    } else {
        fmt.Println("Indeks tim tidak valid.")
    }
}

// Tambah hasil pertandingan dan perbarui klasemen otomatis
func addMatch(aName, bName string, aScore, bScore int) {
    if matchCount < maxMatches {
        matches[matchCount] = Match{TeamA: aName, TeamB: bName, ScoreA: aScore, ScoreB: bScore}
        matchCount++
        // Cari indeks tim
        var ia, ib int = -1, -1
        for i := 0; i < teamCount; i++ {
            if teams[i].Name == aName {
                ia = i
            }
            if teams[i].Name == bName {
                ib = i
            }
        }
        // Update skor
        if ia != -1 && ib != -1 {
            diffA := aScore - bScore
            diffB := bScore - aScore
            teams[ia].ScoreDiff += diffA
            teams[ib].ScoreDiff += diffB
            if aScore > bScore {
                teams[ia].Wins++
            } else if bScore > aScore {
                teams[ib].Wins++
            }
            fmt.Println("Hasil pertandingan berhasil ditambahkan dan klasemen diperbarui.")
        } else {
            fmt.Println("Nama tim tidak ditemukan.")
        }
    } else {
        fmt.Println("Kapasitas pertandingan penuh.")
    }
}

// Tampilkan klasemen
func displayStandings() {
    fmt.Println("--- Klasemen Turnamen ---")
    fmt.Printf("%-3s %-15s %-5s %-5s\n", "No", "Nama Tim", "Menang", "+/-")
    for i := 0; i < teamCount; i++ {
        fmt.Printf("%-3d %-15s %-5d %-5d\n", i+1, teams[i].Name, teams[i].Wins, teams[i].ScoreDiff)
    }
}

// Sequential Search berdasarkan nama
func sequentialSearch(name string) int {
    for i := 0; i < teamCount; i++ {
        if teams[i].Name == name {
            return i
        }
    }
    return -1
}

// Binary Search berdasarkan nama (butuh array terurut secara alfabet)
func binarySearch(name string) int {
    low, high := 0, teamCount-1
    for low <= high {
        mid := (low + high) / 2
        if teams[mid].Name == name {
            return mid
        } else if teams[mid].Name < name {
            low = mid + 1
        } else {
            high = mid - 1
        }
    }
    return -1
}

// Selection Sort berdasarkan jumlah kemenangan
func selectionSortByWins() {
    for i := 0; i < teamCount-1; i++ {
        minIdx := i
        for j := i + 1; j < teamCount; j++ {
            if teams[j].Wins < teams[minIdx].Wins {
                minIdx = j
            }
        }
        teams[i], teams[minIdx] = teams[minIdx], teams[i]
    }
    fmt.Println("Tim diurutkan berdasarkan kemenangan (Selection Sort).")
}

// Insertion Sort berdasarkan selisih skor
func insertionSortByScore() {
    for i := 1; i < teamCount; i++ {
        key := teams[i]
        j := i - 1
        for j >= 0 && teams[j].ScoreDiff > key.ScoreDiff {
            teams[j+1] = teams[j]
            j--
        }
        teams[j+1] = key
    }
    fmt.Println("Tim diurutkan berdasarkan selisih skor (Insertion Sort).")
}

func main() {
    // Data default 3 tim
    addTeam("Alpha")
    addTeam("Bravo")
    addTeam("Charlie")

     var choice int
    for {
        fmt.Println("\nMenu:")
        fmt.Println("1. Tambah/ubah/hapus tim")
        fmt.Println("2. Tambah hasil pertandingan")
        fmt.Println("3. Cari tim (Sequential/Binary)")
        fmt.Println("4. Urutkan tim (Selection/Insertion)")
        fmt.Println("5. Tampilkan klasemen")
        fmt.Println("0. Keluar")
        fmt.Print("Pilih menu: ")
        fmt.Scanln(&choice)

        switch choice {
        case 1:
            fmt.Println("1. Tambah Tim")
            fmt.Println("2. Ubah Tim")
            fmt.Println("3. Hapus Tim")
            fmt.Print("Pilihan: ")
            var op int
            fmt.Scanln(&op)
            var name string
            var idx int
            switch op {
            case 1:
                fmt.Print("Nama tim baru: ")
                fmt.Scanln(&name)
                addTeam(name)
            case 2:
                fmt.Print("Index tim: ")
                fmt.Scanln(&idx)
                fmt.Print("Nama baru: ")
                fmt.Scanln(&name)
                updateTeam(idx-1, name)
            case 3:
                fmt.Print("Index tim: ")
                fmt.Scanln(&idx)
                deleteTeam(idx-1)
            default:
                fmt.Println("Pilihan tidak valid.")
            }

        case 2:
            var aName, bName string
            var aScore, bScore int
            fmt.Print("Nama tim A: ")
            fmt.Scanln(&aName)
            fmt.Print("Nama tim B: ")
            fmt.Scanln(&bName)
            fmt.Print("Skor tim A: ")
            fmt.Scanln(&aScore)
            fmt.Print("Skor tim B: ")
            fmt.Scanln(&bScore)
            addMatch(aName, bName, aScore, bScore)

        case 3:
            var name string
            fmt.Println("Pilih metode pencarian:")
            fmt.Println("1. Sequential Search")
            fmt.Println("2. Binary Search")
            fmt.Print("Pilihan: ")
            var op int
            fmt.Scanln(&op)
            fmt.Print("Nama tim: ")
            fmt.Scanln(&name)
            var res int
            if op == 1 {
                res = sequentialSearch(name)
            } else {
                res = binarySearch(name)
            }
            if res != -1 {
                fmt.Printf("Tim ditemukan di posisi ke-%d\n", res+1)
            } else {
                fmt.Println("Tim tidak ditemukan.")
            }

        case 4:
            var op int
            fmt.Println("Pilih metode pengurutan:")
            fmt.Println("1. Selection Sort (Menang)")
            fmt.Println("2. Insertion Sort (Selisih Skor)")
            fmt.Print("Pilihan: ")
            fmt.Scanln(&op)
            if op == 1 {
                selectionSortByWins()
            } else {
                insertionSortByScore()
            }

        case 5:
            displayStandings()

        case 0:
            fmt.Println("Keluar aplikasi.")
            return
        default:
            fmt.Println("Pilihan tidak valid.")
        }
    }
}
