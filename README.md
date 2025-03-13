## 📝 Performance Testing & Profiling Results

### 🚀 Test Plan Results (Before Optimization)
Sebelum melakukan _optimize code_, saya mengukur waktu respons setiap endpoint menggunakan **Apache JMeter**. Berikut hasilnya:

### 1. Test Plan 1 - `/all-student`
![All Student](src/image/all-student-request.png)

### 2. Test Plan 2 - `/all-student-name`
![All Student Name](src/image/all-student-name.png)

### 3. Test Plan 3 - `/highest-gpa`
![Highest GPA](src/image/highest-gpa.png)

### 🚀 Performance Testing Results (Command Line)

### 1. Test Plan 1 - `/all-student`
![All Student](src/image/test_result_all_student.png)
![All Student Notepad](src/image/test_result_all_student(1).png)

### 2. Test Plan 2 - `/all-student-name`
![All Student Name](src/image/test_result_all_student_name.png)
![All Student Name Notepad](src/image/test_result_all_student_name(1).png)

### 3. Test Plan 3 - `/highest-gpa`
![Highest GPA](src/image/test_result_highest_gpa.png)
![Highest GPA Notepad](src/image/test_result_highest_gpa(1).png)

---

### 🔍 Optimization Strategy
### ✅ Profiling & Bottleneck Identification
Hasil analisis menggunakan **IntelliJ Profiler** :
- **Method `getAllStudentsWithCourses()`** punya CPU time paling tinggi karena melakukan banyak query ke database dalam loop (**N+1 Problem**).
- **Method `joinStudentNames()`** kurang optimal karena menggunakan string concatenation yang berulang.
- **Method `findStudentWithHighestGpa()`** melakukan iterasi terhadap semua data mahasiswa tanpa memanfaatkan query database secara efisien.

### 🔧 Applied Optimizations
- **Menggunakan JOIN FETCH di JPA Queries** untuk mengatasi **N+1 Problem** dan mengurangi jumlah query yang dieksekusi.
- Mengoptimalkan pemrosesan string dengan **`Collectors.joining(", ")`** untuk menghindari overhead dari string concatenation.
- Menggunakan (`@Query` di repository) supaya filtering dan sorting dilakukan langsung oleh database, bukan di dalam code Java.

---

### 🚀 Test Plan Results (After Optimization)
Setelah _optimization code_ dilakukan, saya menjalankan ulang _testing_ dengan **Apache JMeter** untuk melihat peningkatan performa.

### 1. Test Plan 1 - `/all-student` (After Optimization)
![All Student After](src/image/optimized-all-student.png)

### 2. Test Plan 2 - `/all-student-name` (After Optimization)
![All Student Name After](src/image/optimized-all-student-name.png)

### 3. Test Plan 3 - `/highest-gpa` (After Optimization)
![Highest GPA After](src/image/optimized-highest-gpa.png)

### 🚀 Performance Testing Results (Command Line)

### 1. Test Plan 1 - `/all-student` (After Optimization)
![All Student](src/image/result_terminal_optimized_all_student.png)
![All Student Notepad](src/image/result_notepad_optimized_all_student.png)

### 2. Test Plan 2 - `/all-student-name` (After Optimization)
![All Student Name](src/image/result_terminal_optimized_all_student_name.png)
![All Student Name Notepad](src/image/result_notepad_optimized_all_student_name.png)

### 3. Test Plan 3 - `/highest-gpa` (After Optimization)
![Highest GPA](src/image/result_terminal_optimized_highest_gpa.png)
![Highest GPA Notepad](src/image/result_notepad_optimized_highest_gpa.png)

---

### 📈 Perbandingan Sebelum & Sesudah Optimasi
| Endpoint            | Sebelum Optimasi (ms) | Setelah Optimasi (ms) |
|---------------------|-----------------------|-----------------------|
| `/all-student`      | ~301,446 - 305,778    | ~518 - 811            |
| `/all-student-name` | ~4,144 - 4,964        | ~35 - 176             |
| `/highest-gpa`      | ~164 - 344 ms         | ~8 - 30               |

✅ **Target optimization > 20% berhasil dicapai di semua endpoint ^_^**

---