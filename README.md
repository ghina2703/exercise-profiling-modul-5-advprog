# Performance Testing & Profiling Results

## 🚀 Test Plan Results (Before Optimization)

### 1. Test Plan 1 - `/all-student-request`
![All Student Request](all-student-request.png)

### 2. Test Plan 2 - `/all-student-name`
![All Student Name](all-student-name.png)

### 3. Test Plan 3 - `/highest-gpa`
![Highest GPA](highest-gpa.png)

### Performance Testing Results (Command Line)

### 1. Test Plan 1 - `/all-student-request`
![All Student Request](test_result_all_student(1).png)
![All Student Request Notepad](test_result_all_student.png)

### 2. Test Plan 2 - `/all-student-name`
![All Student Name](test_result_all_student_name.png)
![All Student Name Notepad](test_result_all_student_name(1).png)

### 3. Test Plan 3 - `/highest-gpa`
![Highest GPA](test_result_highest_gpa.png)
![Highest GPA Notepad](test_result_highest_gpa(1).png)

---

## 🔍 Optimization Strategy
### ✅ Profiling & Bottleneck Identification
Hasil analisis menggunakan **IntelliJ Profiler** :
- **Method `getAllStudentsWithCourses()`** punya CPU time paling tinggi karena melakukan banyak query ke database dalam loop (**N+1 Problem**).
- **Method `joinStudentNames()`** kurang optimal karena menggunakan string concatenation yang berulang.
- **Method `findStudentWithHighestGpa()`** melakukan iterasi terhadap semua data mahasiswa tanpa memanfaatkan query database secara efisien.

### 🔧 Applied Optimizations
- **Menggunakan JOIN FETCH di JPA Queries** untuk mengatasi **N+1 Problem** dan mengurangi jumlah query yang dieksekusi.
- Mengoptimalkan pemrosesan string dengan **`Collectors.joining(", ")`** untuk menghindari overhead dari string concatenation.
- Menggunakan (`@Query` di repository) supaya filtering dan sorting dilakukan langsung oleh database, bukan di dalam kode Java.

---

## 📊 Test Plan Results (After Optimization)

### 1. Test Plan 1 - `/all-student-request` (After Optimization)
![All Student Request After](optimized-all-student.png)

### 2. Test Plan 2 - `/all-student-name` (After Optimization)
![All Student Name After](optimized-all-student-name.png)

### 3. Test Plan 3 - `/highest-gpa` (After Optimization)
![Highest GPA After](optimized-highest-gpa.png)

### Performance Testing Results (Command Line)
### 1. Test Plan 1 - `/all-student-request` (After Optimization)
![All Student Request](result_terminal_optimized_all_student.png)
![All Student Request Notepad](result_notepad_optimized_all_student.png)

### 2. Test Plan 2 - `/all-student-name` (After Optimization)
![All Student Name](result_terminal_optimized_all_student_name.png)
![All Student Name Notepad](result_notepad_optimized_all_student_name.png)

### 3. Test Plan 3 - `/highest-gpa` (After Optimization)
![Highest GPA](result_terminal_optimized_highest_gpa.png)
![Highest GPA Notepad](result_notepad_optimized_highest_gpa.png)

---

