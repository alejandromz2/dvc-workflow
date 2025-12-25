### Workflow DVC

Principalmente, consiste en definir todos tus hiperparametros en params.yaml.

```yaml
data:
    csv_file_path: ./archive/inicial_data.csv
    test_set_ratio: 0.3
    train_csv_save_path: ./archive/train.csv
    test_csv_save_path: ./archive/test.csv
```


 Cuando terminas de definir todo los hiperparametros de dvc (tus csv, joblib, yaml, etc.), creas un grafo usando dvc.yaml. En este archivo tienes que definir los stages de tu grafo y en cada stage tienes que poner que comando en cmd se va a ejecutar (tus archivos .py), las dependencias que estos tienen, los parametros que definiste en params.yaml y las salidas. Esto se define de la siguiente manera


```yaml
stages:
    initia_state:
        cmd: python ./script.py
        deps:
        - ./script.py
        - ./archive/inicial_data.csv
        params:
        - data 
        outs:
         - ./archive/train.csv
         - ./archive/test.csv
```

Si quieres ver el grafo que has formado, utiliza dvc dag
```bash
dvc dag
```

Y para ejecutar todo tu grafo, usa lo siguiente:
```bash
dvc repro
```

Ten en cuenta, que cuando modifiques algo en un nodo, todos los nodos inferiores se volveran a ejecutar.