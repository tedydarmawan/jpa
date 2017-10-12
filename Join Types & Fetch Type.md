# Join Types
Ada 4 tipe join:
- @OneToOne
- @OneToMany
- @ManyToOne
- @ManyToMany

Dapat digunakan di beberapa konfigurasi:
- Unidrectional
- Bidirectional
- Cascade

## @OneToOne
- Asumsi: 1 order hanya memiliki 1 invoice dan sebaliknya
- View diagram: https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1#G0B2Ue1t6hHpWJenFpelJaMlFBZ0U

Kelas Invoice.java
``` java
@OneToOne
@JoinColumn(name = "ORDER_ID") 
private Order order;
```

Kelas Order.java
``` java
@OneToOne(mappedBy="order")
private Invoice invoice;
```

## @OneToMany
- Asumsi: 1 distributor mempunyai banyak role
- View diagram: https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1#G0B2Ue1t6hHpWJYUE4RmdiMjZCLTg

Kelas Distributor.java
``` java
@OneToMany(mappedBy="distributor", fetch=FetchType.LAZY)
private List<Role> roles = new ArrayList<>();
```

## @ManyToOne
- Asumsi: 1 role hanya memiliki 1 distributor
- View diagram: https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1#G0B2Ue1t6hHpWJYUE4RmdiMjZCLTg

Kelas Role.java
``` java
@ManyToOne
@JoinColumn(name = "DISTRIBUTOR_ID", referencedColumnName = "DISTRIBUTOR_ID")
private Distributor distributor;
```

## @ManyToMany
- Asumsi 1 role mempunyai banyak menu dan 1 menu mempunyai banyak role
- View Diagram: https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1#G0B2Ue1t6hHpWJSzgyQXhzSmtGNXM

Kelas Role.java
``` java
@ManyToMany(fetch=FetchType.EAGER)
@JoinTable(name = "TROLE_MENU", 
  joinColumns = {
	  @JoinColumn(name = "ROLE_ID", referencedColumnName = "ROLE_ID") 
	}, 
	inverseJoinColumns = {
		@JoinColumn(name = "MENU_ID", referencedColumnName = "MENU_ID") 
	}
)
private List<Menu> menus = new ArrayList<>();
```

Kelas Menu.java
``` java
@ManyToMany(mappedBy = "menus")
private List<Role> roles;
```

## Fetch Type
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


