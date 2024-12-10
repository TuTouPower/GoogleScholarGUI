# GoogleScholarGUI
谷歌学术爬虫GUI版本。    
该软件仅用来演示爬取谷歌的技术，不可用于非法的爬虫用途，由于不当使用该软件产生的一切法律纠纷均与本人无关。    
本程序可以爬取到的字段示意：    
![爬取字段](./images/爬取的字段.png)
## 一、开发环境
**Python版本：**：Python 3.7    
**GUI框架**：PyQt5    
**使用到的其他依赖**：    
- requests    
- requests[socks]    
- lxml    
- pandas    
- retrying    
- xlrd    

## 二、使用教程
1. 安装版本直接双击即可安装，安装完成后双击打开GoogleScholar.exe，就可以看到以下界面：
![界面](./images/界面.png)
2. 搜索设置：
在搜索设置区域根据要搜索的内容设置合适的参数，例如：
![搜索设置](./images/搜索设置.png)
3. 导出设置：
如果要将PDF链接导出到一个单独的txt文件请勾选导出PDF链接到文件复选框并选择PDF链接存储文件，导出链接后可以导入第三方的批量下载软件进行快速下载    
如果要自动下载PDF文件请勾选自动下载PDF复选框并选择PDF保存路径，软件会将PDF下载到指定的文件夹中。    
下载线程数可以根据自己的电脑性能合理分配。    
4. 防反爬设置：
谷歌为了防止爬虫，采取了很多反爬措施，如果不进行防反爬处理只能爬到很少的文献就被谷歌识别为爬虫代码，主要设置以下三种防反爬：随机agents，随机谷歌域名和IP代理池，根据自己的需要选择相应的文件并勾选后面的启用就可以设置完成。此项非专业人士请勿自行设置。如果不想使用IP代理池和随机agents可以不勾选复选框，需要配置config.txt文件（config.txt文件放在安装目录下的config文件夹中），如下图：
![config](./images/config.png)
该文件一共包含三行内容，第一行为代理地址，不使用IP代理池时必须设置要不然国内访问不了谷歌学术；第二行为agents设置，不使用随机agents时设置，不需要设置时需要留空。第三行为谷歌的cookie，可以选择性设置，不设置时第三行留空；    
1. 开始爬取，设置好以上参数就可以点击开始按钮开始爬虫了，软件将会自动运行。
注意：编译好的exe可能存在一些兼容性问题导致开始爬虫后没有反应，可以直接下载源码运行Interface.py启动软件进行操作（需要自己安装Python并配置依赖）。
## 三、目前存在的问题
1. PDF下载功能存在缺陷，有些链接不能正常下载，多线程下载功能还在开发中，后续会不断完善；
2. 在论文详情解析的过程中，由于没有对一些数据库做适配，可能出现有写论文的详情爬取不完整的情况。
3. 潜在的Bug正在寻找与完善中，欢迎大家提出改进的意见和建议。

# Interface.py

## 1. 文件概述
这是一个基于 PyQt5 开发的 Google Scholar 爬虫程序的图形界面实现，提供了完整的用户交互界面和爬虫控制功能。

## 2. 主要类

### 2.1 MainWindow 类
```python
class MainWindow(QtWidgets.QMainWindow):
```
- 继承自 QtWidgets.QMainWindow
- 实现了程序关闭确认对话框
- 处理窗口关闭事件

### 2.2 Ui_Widget 类
```python
class Ui_Widget(QtWidgets.QMainWindow):
```
- 主界面实现类
- 包含所有 UI 元素和功能实现
- 管理爬虫线程和控制逻辑

## 3. 界面布局

### 3.1 搜索设置区域
```python
def setupUi(self, MainWindow):
    # 搜索内容输入框
    self.lineEditSearchContent
    # 爬取页数设置
    self.spinBoxSearchPages
    # 各种复选框
    self.checkBoxPatent      # 包含专利
    self.checkBoxQuote      # 包含引用
    self.checkBoxDetails    # 爬取详情
```

### 3.2 导出设置区域
```python
# 导出选项
self.checkBoxSavePDFLink    # 导出PDF链接
self.checkBoxDownloadPDF    # 下载PDF
self.spinBoxDownloadThreads # 下载线程数
```

### 3.3 防反爬设置区域
```python
# 反爬虫配置
self.lineEditAgents     # User-Agent 文件
self.lineEditDomains    # 域名文件
self.lineEditIPs        # 代理IP文件
```

## 4. 核心功能实现

### 4.1 爬虫控制
```python
def start(self):
    # 启动爬虫线程
    # 处理暂停/继续功能
    # 管理爬虫状态

def start_crawler(self):
    # 爬虫主要逻辑
    # URL构建
    # 数据获取和解析
    # 结果保存
```

### 4.2 文件选择功能
```python
def selectResultSaveFile(self)    # 选择结果保存文件
def selectLinkSaveFile(self)      # 选择链接保存文件
def selectPDFSavePath(self)       # 选择PDF保存路径
def selectAgents(self)            # 选择UA文件
def selectDomains(self)           # 选择域名文件
def selectIPs(self)               # 选择代理IP文件
```

### 4.3 线程控制
```python
def pause(self):      # 暂停爬虫
def resume(self):     # 继续爬虫
def stop(self):       # 停止爬虫
```

## 5. 特色功能

### 5.1 多线程支持
```python
pauseFlag = threading.Event()     # 暂停标识
exitFlag = threading.Event()      # 退出标识
updated = QtCore.pyqtSignal(str)  # 更新信号
```

### 5.2 进度显示
```python
# 进度条更新
self.progressBar.setValue(int(progressBarValue))

# 日志更新
self.updated.emit('日志信息')
```

### 5.3 配置管理
```python
# 读取配置文件
with open('./config/config.txt', 'r', encoding='utf-8') as proxy_file:
    # 读取代理设置
    # 读取UA设置
    # 读取Cookie设置
```

## 6. 界面交互

### 6.1 按钮事件
```python
# 按钮点击事件绑定
self.pushButtonStart.clicked.connect(self.start)
self.pushButtonCancel.clicked.connect(self.stop)
```

### 6.2 复选框联动
```python
def setSavePDFLink(self):    # PDF链接导出设置
def setSavePDF(self):        # PDF下载设置
def setAgents(self):         # UA设置
def setDomains(self):        # 域名设置
def setIPs(self):            # 代理设置
```

## 7. 使用示例

```python
if __name__ == '__main__':  
    app = QtWidgets.QApplication(sys.argv)
    MainWindow = MainWindow()
    ui = Ui_Widget()
    ui.setupUi(MainWindow)
    MainWindow.show()
    sys.exit(app.exec_())
```

这个文件实现了一个完整的图形界面程序，提供了：
1. 直观的用户界面
2. 完整的爬虫控制功能
3. 灵活的配置选项
4. 多线程支持
5. 实时进度显示
6. 详细的日志输出

是一个功能完善的 Google Scholar 爬虫程序的界面实现。

# GoogleScholar.py

## 1. 文件概述
这是一个 Google Scholar 学术文献爬虫程序的核心实现文件，主要用于从 Google Scholar 抓取学术文献的相关信息。

## 2. 主要功能
- 抓取论文基本信息（标题、作者、期刊等）
- 获取论文引用次数
- 提取论文摘要
- 获取 PDF 下载链接
- 支持多种学术网站的详细信息抓取
- 结果导出到 Excel

## 3. 代码结构

### 3.1 配置文件读取函数
```python
def read_agents(filename)    # 读取 User-Agent 列表
def read_domains(filename)   # 读取谷歌域名列表
def read_ips(filename)      # 读取代理 IP 列表
```

### 3.2 代理设置函数
```python
def select_proxy(proxy_type='socks-client')
```
支持四种代理模式：
- http: HTTP/HTTPS 代理
- socks-remote: 远程 SOCKS5 代理
- socks-client: 本地 SOCKS5 代理
- no: 不使用代理

### 3.3 网页数据获取
```python
@retry(stop_max_attempt_number=5, wait_random_min=10, wait_random_max=20)
def get_data(url, headers, proxies)
```
特点：
- 使用装饰器实现自动重试
- 支持 gzip 压缩响应处理
- 包含 cookie 处理
- 内置垃圾回收机制

### 3.4 数据解析函数
```python
def parse_data(html, page_count, detailFlag)
```
主要解析内容：
1. 基础信息：
   - 标题 (Title)
   - 期刊 (Journal)
   - 作者 (Authors)
   - 年份 (Year)
   - 引用次数 (Citation)
   - 摘要 (Abstract)
   - PDF 链接

2. 支持的详细信息源：
   - ScienceDirect
   - SAGE Journal
   - Wiley
   - Springer
   - Emerald

### 3.5 数据导出函数
```python
def sava_to_excel(article_list, file_path)
```
功能：
- 将抓取结果转换为 DataFrame
- 指定字段顺序
- 支持追加写入
- 自动处理空值

### 3.6 时间计算函数
```python
def cal_time(pages)
```
计算爬取大致所需时间

## 4. 反爬虫策略

### 4.1 随机延时
```python
delay_time = random.randint(25,35)    # 随机延时25-35秒
time.sleep(delay_time)
```

### 4.2 请求头处理
- 随机 User-Agent
- 支持 gzip 压缩
- Cookie 管理

### 4.3 代理切换
- 支持多种代理模式
- IP 池轮换
- 域名轮换

## 5. 详细信息抓取策略

### 5.1 ScienceDirect
```python
if "sciencedirect" in paper_link[0]:
    # 特殊请求头设置
    # 提取完整作者信息
    # 提取完整期刊信息
    # 提取完整摘要
```

### 5.2 Springer
```python
elif "springer" in paper_link[0]:
    # 使用新代理
    # 提取作者列表
    # 格式化作者名称
    # 提取完整摘要
```

### 5.3 Wiley
```python
elif "wiley" in paper_link[0]:
    # 提取作者信息
    # 格式化作者名称
    # 提取文章摘要
```

### 5.4 Emerald
```python
elif "emerald" in paper_link[0]:
    # 处理作者信息
    # 清理摘要格式
    # 移除多余空白
```

## 6. 内存管理

### 6.1 垃圾回收
```python
# 主动释放内存
del agents,agent,headers,proxies
gc.collect()
```

### 6.2 文件处理
```python
# 确保文件正确关闭
agents_file.close()
del agents_file
gc.collect()
```

## 7. 异常处理

### 7.1 网络请求异常
```python
try:
    response = request.urlopen(req)
except urllib.error.URLError as e:
    print('GoogleScholar.get_data()：' + str(e.reason))
```

### 7.2 数据解析异常
```python
try:
    year = re.findall(r'\b\d{4}\b', authors_journal_year_site)[0]
except Exception:
    year = '0'
```

## 8. 性能优化

### 8.1 重试机制
```python
@retry(stop_max_attempt_number=5, wait_random_min=10, wait_random_max=20)
```

### 8.2 数据缓存
- 使用字典存储中间结果
- DataFrame 批量处理
- 文件追加写入

## 9. 使用示例

```python
# 1. 初始化配置
agents = read_agents('./config/agents.txt')
domains = read_domains('./config/domains.txt')

# 2. 设置代理
proxies = select_proxy('socks-client')

# 3. 获取数据
data = get_data(url, headers, proxies)

# 4. 解析数据
article_list = parse_data(html, page_count, detailFlag=True)

# 5. 导出结果
sava_to_excel(article_list, 'results.xlsx')
```

这个文件实现了一个完整的学术文献爬虫系统，包含了反爬虫策略、异常处理、性能优化等重要特性，能够稳定地从 Google Scholar 和各大学术网站抓取文献信息。

# PDFDownload.py

## 1. 文件概述
这是一个用于下载学术论文 PDF 文件的模块，主要功能包括从 Excel 文件读取论文信息、下载 PDF 文件、错误处理和重试机制。

## 2. 主要功能

### 2.1 Excel 文件读取
```python
def read_file(filename):
    # 从Excel读取论文信息
    # 返回包含论文信息的字典列表
```

功能特点：
- 使用 pandas 读取 Excel 文件
- 提取论文的关键信息
- 包含内存管理机制
- 返回标准化的数据结构

### 2.2 PDF 文件下载
```python
def get_file(article, path):
    # 下载单个PDF文件
    # 处理下载异常
    # 返回错误信息
```

实现细节：
1. URL 处理：
```python
url = article['PDF Link']
filename = '/' + article['Title']
```

2. 文件下载：
```python
data = urllib.request.urlopen(url)
pdf_file = open(path + filename + '.pdf', 'wb')
```

3. 分块下载：
```python
block_sz = 8192
while True:
    buffer = data.read(block_sz)
    if not buffer:
        break
    pdf_file.write(buffer)
```

### 2.3 批量下载管理
```python
def download_file(Ui_Widget, article_list, path):
    # 管理批量下载任务
    # 支持暂停/继续功能
    # 错误文件记录
```

主要特性：
- 支持线程控制
- 错误文件记录
- 进度跟踪
- 内存管理

## 3. 数据结构

### 3.1 文章信息字典
```python
article_dict = {
    'Title': title,
    'Journal': journal,
    'Authors': authors,
    'Year': year,
    'Citation': citation,
    'Abstract': abstract,
    'PDF Link': pdf_link
}
```

### 3.2 错误记录
```python
error_article_list = []  # 记录下载失败的文章
```

## 4. 错误处理

### 4.1 URL 打开错误
```python
try:
    data = urllib.request.urlopen(url)
except Exception:
    print('URL打开失败，继续下载下一个。')
    error_article = deepcopy(article)
```

### 4.2 文件下载错误
```python
try:
    # 文件下载逻辑
except Exception:
    print('下载出错，继续下载下一个。')
    error_article = deepcopy(article)
```

## 5. 内存管理

### 5.1 资源释放
```python
def cleanup():
    pdf_file.close()
    data.close()
    del data
    gc.collect()
```

### 5.2 变量清理
```python
del dataframe
del rows
del title
del article_dict
gc.collect()
```

## 6. Excel 导出功能

```python
def sava_to_excel(article_list, filename):
    # 将数据转换为DataFrame
    dataframe = pd.DataFrame(list(article_list))
    
    # 指定列顺序
    order = ['Title', 'Journal', 'Authors', 'Year', 
             'Citation', 'Abstract', "PDF Link"]
    
    # 处理空值
    dataframe.fillna(' ',inplace = True)
    
    # 支持追加写入
    if os.path.isfile(file_path):
        dataframe = pd.read_excel(file_path).append(
            dataframe, ignore_index=True, sort=False)
```

## 7. 使用示例

```python
# 1. 读取论文信息
article_list = read_file("papers.xlsx")

# 2. 设置下载路径
download_path = "./downloads"

# 3. 开始下载
download_file(ui_widget, article_list, download_path)

# 4. 错误处理
if error_article_list:
    sava_to_excel(error_article_list, 'Errors.xlsx')
```

这个模块提供了完整的 PDF 下载功能，包括：
1. 文件信息读取
2. 批量下载管理
3. 错误处理和重试
4. 内存管理优化
5. 进度跟踪
6. 错误记录导出

是一个功能完善的学术论文 PDF 下载工具。
