# 2 ПГ, ДЗ 1

## 1. Анализ QC прочтений
Отчет находится в директории reports.
Ответ на вопрос: мы можем наблюдать значительное преобладание GC нуклеотидов в сравнении с остальными (per sequence GC content). Учитывая то, что для qc отчета нами был выбра образец, для которого, по нашим предположениям, увеличивается уровень метилирования, наблюдаемая картина подтверждает наши предположения.  
  
## 2. 
### 1) Количество ридов, закартированных на конкретные участки для каждого образца:  
| Образец   |11347700-11367700|40185800-40195800|
|:---------:|:---------------:|:---------------:|
| cell8     | 1090            | 464             |
| epiblast  | 2328            | 1062            |
| ICM       | 1456            | 630             |

### 2) Дедупликация файлов  
|Образец |Кол-во дупликаций|Процент Дупликаций|
|:------:|:---------------:|:----------------:|
|cell8   |521904           |18.31%            |
|epiblast|205258           |2.92%             |
|ICM     |377882           |9.08%             |

Скрипт для параллельного выполнения команд: `ls *pe.bam | xargs -P 4 -tI{} deduplicate_bismark  --bam  --paired  -o s_{} {}`  

### 3) Коллинг метилирование
Done

### 4) Отчет
Отчеты также находятся в директории reports.  
Скриншот:  
![image](https://user-images.githubusercontent.com/55440084/154321806-ecdb0b88-01d9-4aab-a85a-29577ef5c259.png)
![image](https://user-images.githubusercontent.com/55440084/154321910-5d22383f-a77a-4b34-b87c-a10cf1275d32.png)
![image](https://user-images.githubusercontent.com/55440084/154321964-a3454593-802e-4f6f-b574-77d510393b7f.png)

Описание: из документации сладует, что графики показывают долю метелированиях на всех позициях хромосомы. Можно пронаблюдать, что больше всего метилирования происходит в образце Epiblast, значительно меньше в 8Cell, и совсем мало в ICM.
  
### 5) Гистограмы распределения метилирования цитозинов по хромосоме

Epiblast:  
![SRR3824222](https://user-images.githubusercontent.com/55440084/154324757-b54c330d-4f29-4bd6-9ad3-12fd10daf8eb.png)  

ICM:  
![SRR5836475](https://user-images.githubusercontent.com/55440084/154324818-20e49dbc-81a9-45c1-be1c-13460301c7ff.png)  

8Cell:  
![SRR5836473](https://user-images.githubusercontent.com/55440084/154324850-49a28dee-b2b5-4acd-aa73-afd86b21e5d0.png)  

Вывод: для образца Epiblast мы наблюдаем максимальный уровень метилирования днк, в этом случае метилируется большая часть хромосомы. В образце ICM метилирования практически не происходит, а в 8Cell - имеет место быть, но не так значительно, как в Epiblast. Т.е. мы видим, что на разных этапах развития эмбриона, уровень метилирования действительно меняется.  

Код:  
```
names = ['5836473', '5836475', '3824222']
for name in names:
  data = pd.read_csv('s_SRR'+name+'_1_bismark_bt2_pe.deduplicated.bedGraph', delimiter='\t', skiprows=1, header=None)
  fig = plt.figure()
  plt.title('SRR'+name+' methylation distribution')
  plt.hist(data[3], bins=100, density=True)
  plt.xlabel('Percentage of methylated cytosines')
  plt.ylabel('Frequency')
  plt.show()
  fig.savefig('SRR'+name+'.png')
  files.download('SRR'+name+'.png')
```

### 6) Визуализация уровня метилирования и покрытия
Уровень метилирования:  
![meth_lvl](https://user-images.githubusercontent.com/55440084/154295650-656d4bc3-a251-4615-8969-63da365a51e5.png)
   
Уровень покрытия:  
![cov](https://user-images.githubusercontent.com/55440084/154295680-f7def500-2849-4409-870a-ea3286d0c3f5.png)
