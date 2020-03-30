ข้อนี้เป็นการฝึก implementation สมมติว่าเรารับความสูงของภูเขามาอยู่ใน `a[i]` แล้ว

```cpp
vector<int> peaks;
for(int i = 0; i< n; i++)
{
    if((i+1>= n || a[i]> a[i+1]) && (i-1< 0 || a[i]> a[i-1]))
    {
        peaks.push_back(a[i]);
    }
}
```
ตอนนี้ `peaks` จะเก็บความสูงของภูเขาเด่นทั้งหมด ต่อไปจะเป็นการเรียงลำดับและลบความสูงซ้ำออกไป

```cpp
sort(peaks.begin(), peaks.end());
peaks.resize(unique(peask.begin(), peaks.end())-peaks.begin());
reverse(peaks.begin(), peaks.end());
```

ตอนนี้ `peaks` จะเก็บความสูงเรียงลำดับจากมากไปน้อยและจะไม่มีความสูงที่ซ้ำกันเลย

จากนั้นจะเป็นการแสดงคำตอบ

```cpp
for(int i = 0; i< min(k, peaks.size()); i++)
{
    cout << peaks[i] << endl;
}
```

โปรแกรมนี้จะทำงานในเวลา $\mathcal{O}(nlogn)$