# Photoelasticity Image Dataset

## Dataset Overview
This dataset contains photoelasticity images generated using random forces, with a resolution of **224x224**. The data of each image corresponds to physical properties at specific nodes (including angles, pressure, and friction). The labels are sorted based on the size of the angles.

The dataset is divided into **training** and **testing** sets, with corresponding CSV files containing labels for each image.

---

## Dataset File Descriptions
The dataset contains the following files:

- **Training Data**
  - `train/`: Folder containing the training images.
  - `train_labels.csv`: CSV file for the training set labels, describing the node properties (angles, pressure, friction) for each image.
  
- **Testing Data**
  - `test/`: Folder containing the test images.
  - `test_labels.csv`: CSV file for the testing set labels, describing the node properties (angles, pressure, friction) for each image.

#### CSV File Column Descriptions:
| Column | Description                |
|--------|----------------------------|
| `0`    | Angle at node 0            |
| `1`    | Pressure at node 1         |
| `2`    | Friction at node 2         |
| `10`   | Angle at node 10           |
| `11`   | Pressure at node 11        |
| `12`   | Friction at node 12        |
| `20`   | Angle at node 20           |
| `21`   | Pressure at node 21        |
| `22`   | Friction at node 22        |

---

## Image File Descriptions
- Each image file name corresponds to its label row number.
- Image resolution: **224x224**.
- Image format: `.jpg` or `.png` (depending on the uploaded files).

---

## Dataset Structure Example
#### Training Set Folder Structure:
```plaintext
train/
├── 0.jpg
├── 1.jpg
├── 2.jpg
...
train_labels.csv
test/
├── 0.jpg
├── 1.jpg
├── 2.jpg
...
```
---
## How to Use the Dataset
### **1.Download the Dataset: Clone the repository and unzip the dataset:**
git clone https://github.com/your-repo/dataset.git

### **2.Loading Images and Labels: **
Use the following code to load images and their corresponding labels:

```python
class Mydata(Dataset):
    
    def __init__(self,root, train=True , transform = None , target_transform=None , num_data=None):
        super(Mydata,self).__init__()
        self.train = train
        self.transform = transform
        self.target_transform = target_transform
        
       
        if self.train:
            file_annotation = root+'/annotation/train.csv' # 修改文件位置
            img_folder = root + '/train_rot/'
        else:
            file_annotation = root+'/annotation/test.csv' #修改文件位置
            img_folder = root + '/test/'
            
        self.label = pd.read_csv(file_annotation,sep=',',index_col=['name'])
        
        if num_data == None:
            self.num_data = len(self.label)
        else:
            self.num_data=num_data
       
        
        self.filenames = []
        #self.labels = []
        self.img_folder = img_folder
        
        for i in range(self.num_data):
            self.filenames.append(f"{i}.jpg")
            #selflf.labels.append()
            
    def __getitem__(self,index):
        img_name = self.img_folder + self.filenames[index]
       
        label = self.label.loc[index][:]
   
        label = np.array(label)
        label = torch.tensor(label)
        label = label.float()
        img = plt.imread(img_name)
      
        img = self.transform(img)
        
        return img,label
    
    def __len__(self):
        return len(self.filenames)
```
### **3.Train a Model:** You can use frameworks like PyTorch or TensorFlow to train models using this dataset.

## **Contact Information**

For any inquiries, please contact: 1095483213@qq.com
