# Ansible變數

[toc]

# 變數寫在哪？
事實上，變數寫在**任何地方**都可以，也就是說無論是`inventory.yml`、`playbook.yml`甚至可以獨立寫一個`vars.yml`這種YAML檔案從外部引入。對，正如前面舉例的工通典，你可以發現都是YAML檔案符合YAML格式才能作為變數使用，所以像是`inventory.ini`無法寫入變數。

# 變數的使用
我們以執行`whoami`為例子，`whoami`這種output類指令需要儲存執行結果然後搭配`debug`這類印出資訊的模組才可以得到執行結果，我寫了一個`whoami`的Playbook示範：
```YAML
- name: whoami 1
  hosts: myhosts
  vars_files:
    - su_root_vault.yml
  tasks:
    - name: whoami command
      command: whoami
      register: result

    - name: Print result
      debug:
        var: result.stdout
```
`command`有一個參數叫做`register`，可以用來紀錄指令執行結果，同時你可以注意到我引入了一個外部變數檔，用來提權，相關的用法可以參考[become提權](./become提權.md)，這裡主要展示`vars_file`如何使用

你可以看到我用`register`紀錄結果後，我在`debug`用`var`把紀錄的結果透過變數導入進去，同時我加入了`.stdout`代表標準系統輸出

# Jinja2 格式
Ansible除了依賴YAML以外，它同時也依賴**Jinja2**的語法，也就是說你也可以透過Jinja2的語法來用作變數，比如下面這個Playbook，示範了Jinja2格式的變數和`include_vars`的用法：
```YAML
- name: whoami 2
  hosts: myhosts
  tasks:
    - name: whoami (user)
      command: whoami
      register: result1

    - name: include vars
      include_vars:
        file: su_root_vault.yml
      
    - name: whoami (root)
      command: whoami
      register: result2

    - name: print result
      debug:
        msg: "result1: {{ result1.stdout }} , result2: {{ result2.stdout }}"
```
`{{ vars }}`這種用法就是Jinja2的用法，Ansible當然還支持其他Jinja2的語法，但會搭配`Template`模組來使用，我留到[Template基礎](../Template_Jinja2/Template基礎.md)再說。

`include_vars`不同之處在於它是在Tasks階段使用，並且是使用後後續的Tasks都會受到影響，這個Playbook很好的展示了不同時機引入變數會怎麼影響執行結果。就優先及來說，`include_vars`的優先度比`vars_file`還高，因為是後面更改的變數。

透過`vars`宣告的變數一般來說會比較適合用於指定執行環境，而`{{ vars }}`則是需要靈活搭配執行狀態決定執行結果，所以2者的用途不一樣，沒有誰更好，需要根據需求靈活運用。

# list
學會一般的變數如何宣告與使用後，我們來學學如何宣告list，在`vars`底下，list可以用這兩種宣告方法：
```YAML
# Methods 1:
vars:
  a_list: [ 1, 2, 3, "str1", "str2" ]

# Medhods 2:
vars:
  a_list:
  - 1
  - 2
  - 3
  - "str1"
  - "str2"

```
是的，跟Python一樣，可以數字和字串混合宣告，而且若要取出list的單一值，跟Python一樣，使用`a_list[nums]`來取得，從0開始，比如以下寫法：
```YAML
debug:
  msg: "I selected index 2 {{ a_list[1] }} "
```

# distionary


# loops的使用
**loops**模組的使用也十分依賴變數，但它依賴的是**list**，Ansible也可以使用**dict**在loops內，搭配list來使用

如果看過比較早期的Ansible Docs或別人整理的筆記，你可能看過`with_items`，這個是舊版語法，新版語法用的是`loop`模組，使用方法是在需要重複執行的模組下面使用`loop`模組，並且指定使用的list，比如：
```YAML
debug: 
  msg: "Printed {{ item }}"
loop: "{{ a_list }}"
```
沒有錯，`loop`跟上面的模組視同一層，並且丟回去的變數使用`item`來接收，但還有一種情況是我沒宣告變數，而是要用固定的一個數字範圍呢？很簡單，使用`range`，如下：
```YAML
debut:
  msg: "Printed {{ item }}"
loop: "{{ range(STR, END) | list }}"
```
`range`後面的`list`是用來把前面的`range`所指定的變數範圍轉變為list，另外`range`的語法跟Python一樣，可以使用`range(STR, END, STEP)`，比如如下：
```YAML
debut:
  msg: "Printed {{ item }}"
loop: "{{ range(STR, END, STEP) | list }}"
```
這樣你就會基本的`loop`用法了，但你知道，loop可以搭配dict嗎？


# Reference
[Ansible Docs - Using Variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)
[Ansible Docs - Dictionaries](https://docs.ansible.com/ansible/latest/collections/community/general/docsite/filter_guide_abstract_informations_dictionaries.html)
[Ansible Docs - Loops](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html)