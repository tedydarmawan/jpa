# Join Types
Ada 4 tipe join:
- @OneToOne
- @OneToMany, digunakan untuk mendefinisikan relasi diantara suatu Object dengan List Object
``` java
@OneToMany(mappedBy="goal", cascade=CascadeType.ALL)
private List<Exercise> exercises = new ArrayList<Exercise>();
```
- @ManyToOne
- @ManyToMany

Dapat digunakan dibeberapa konfigurasi:
- Unidrectional
- Bidirectional
- Cascade

# Fetch Type
- Lazy, Query ke database dilakukan ketika property dipanggil
``` java
@OneToMany(mappedBy="goal", cascade=CascadeType.ALL, fetch=FetchType.LAZY)
private List<Exercise> exercises = new ArrayList<Exercise>();
```
- Eager, Query ke database dilakukan pada saat objek dibuat
``` java
@OneToMany(mappedBy="goal", cascade=CascadeType.ALL, fetch=FetchType.EAGER)
private List<Exercise> exercises = new ArrayList<Exercise>();
```


