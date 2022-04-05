### run the baseline experiment with MNIST on MLP

运行

```Python
python src/baseline_main.py --model=mlp --dataset=mnist --gpu=cuda:0 --epochs=10
```

结果如下

![image-20220227122800990](images/FedAVG/image-20220227122800990.png)

<img src="images/FedAVG/image-20220227123110632.png" alt="image-20220227123110632" style="zoom: 50%;" />

<img src="images/FedAVG/image-20220227123052092.png" alt="image-20220227123052092" style="zoom:50%;" />

<img src="images/FedAVG/image-20220227123128423.png" alt="image-20220227123128423" style="zoom:50%;" />

### run the federated experiment with CIFAR on CNN (IID):

```Python
python src/federated_main.py --model=cnn --dataset=cifar --gpu=0 --iid=1 --epochs=10
```

Train Accuracy仅仅是40%

<img src="images/FedAVG/image-20220227134008400.png" alt="image-20220227134008400" style="zoom:50%;" />

把epoch改成50

```Python
python src/federated_main.py --model=cnn --dataset=cifar --gpu=cuda:0 --iid=1 --epochs=50
```

好了一点

<img src="images/FedAVG/image-20220227140922409.png" alt="image-20220227140922409" style="zoom:50%;" />

把epoch改成500

![image-20220228182744533](images/FedAVG/image-20220228182744533.png)

![image-20220301101939153](images/FedAVG/image-20220301101939153.png)
