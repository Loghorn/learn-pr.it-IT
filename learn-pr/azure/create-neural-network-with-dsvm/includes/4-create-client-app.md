### <a name="create-a-nothotdog-app"></a><span data-ttu-id="c58aa-101">Creare un'app NotHotDog</span><span class="sxs-lookup"><span data-stu-id="c58aa-101">Create a NotHotDog app</span></span>

<span data-ttu-id="c58aa-102">In questa unità si userà [Visual Studio Code](https://code.visualstudio.com/), l'editor di codice sorgente multipiattaforma gratuito di Microsoft, preinstallato nella Data Science VM, per scrivere un'app NotHotDog in Python.</span><span class="sxs-lookup"><span data-stu-id="c58aa-102">In this unit, you will use [Visual Studio Code](https://code.visualstudio.com/), Microsoft's free, cross-platform source-code editor which is preinstalled in the Data Science VM, to write a NotHotDog app in Python.</span></span> <span data-ttu-id="c58aa-103">L'app userà [TkInter](https://wiki.python.org/moin/TkInter), un noto framework GUI per Python, per implementare l'interfaccia utente e consentirà di selezionare le immagini dal file system locale.</span><span class="sxs-lookup"><span data-stu-id="c58aa-103">The app will use [Tkinter](https://wiki.python.org/moin/TkInter), which is a popular GUI framework for Python, to implement its user interface, and it will allow you to select images from your local file system.</span></span> <span data-ttu-id="c58aa-104">Le immagini verranno quindi passate al modello sottoposto a training nell'esercizio precedente e l'app indicherà se contengono un hot dog.</span><span class="sxs-lookup"><span data-stu-id="c58aa-104">Then, it will pass those images to the model you trained in the previous exercise and tell you whether they contain a hot dog.</span></span>

1. <span data-ttu-id="c58aa-105">Fare clic su **Applicazioni** nell'angolo superiore sinistro del desktop e selezionare **Accessori > Visual Studio Code** per avviare Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c58aa-105">Click **Applications** in the upper-left corner of the desktop and select **Accessories > Visual Studio Code** to start Visual Studio Code.</span></span> <span data-ttu-id="c58aa-106">Usare il comando di Visual Studio Code **File > Apri cartella** per aprire la cartella "notebooks/tensorflow-for-poets-2/tf_files" contenente il file **retrained_graph_hotdog.pb** creato durante il training del modello.</span><span class="sxs-lookup"><span data-stu-id="c58aa-106">Use Visual Studio Code's **File > Open Folder...** command to open the "notebooks/tensorflow-for-poets-2/tf_files" folder containing the **retrained_graph_hotdog.pb** file created when you trained the model.</span></span>

1. <span data-ttu-id="c58aa-107">Creare un nuovo file denominato **classify.py** nella cartella corrente.</span><span class="sxs-lookup"><span data-stu-id="c58aa-107">Create a new file named **classify.py** in the current folder.</span></span> <span data-ttu-id="c58aa-108">Se Visual Studio Code offre di installare l'estensione di Python, fare clic su **Installa** per installarla.</span><span class="sxs-lookup"><span data-stu-id="c58aa-108">If Visual Studio Code offers to install the Python extension, click **Install** to install it.</span></span> <span data-ttu-id="c58aa-109">Copiare il codice seguente negli Appunti e usare **MAIUSC+INS** per incollarlo in **classify.py**.</span><span class="sxs-lookup"><span data-stu-id="c58aa-109">Copy the code below to the clipboard and use **Shift+Ins** to paste it into **classify.py**.</span></span> <span data-ttu-id="c58aa-110">Salvare quindi il file.</span><span class="sxs-lookup"><span data-stu-id="c58aa-110">Then, save the file:</span></span>

    ```python
    import tkinter as tk
    from tkinter import messagebox, filedialog, font
    from PIL import ImageTk, Image
    import subprocess

    def select_image_click(img_label):
        try:
            file = filedialog.askopenfilename()

            img = Image.open(file)
            img = img.resize((300, 300))
            selected_img = ImageTk.PhotoImage(img)

            img_label.configure(image=selected_img, width=240)

            output = subprocess.check_output(["python",
                "../scripts/label_image.py",
                "--graph=retrained_graph_hotdog.pb",
                "--image={0}".format(file),
                "--labels=retrained_labels_hotdog.txt"])

            highest = str(output).split("\\n")[3].split(" ")

            if len(highest) == 3:
                score = float(highest[2])
                is_hotdog = True
            else:
                score = float(highest[3])
                is_hotdog = False

            if score > 0.95:
                if is_hotdog:
                    messagebox.showinfo("Result", "That's a hot dog!")
                else:
                    messagebox.showinfo("Result", "That's not a hot dog.")
            else:
                messagebox.showinfo("Result", "Can't tell.")

        except FileNotFoundError as e:
            messagebox.showerror("File not found", "File {0} was not found.".format(e.filename))

    def run():
        window = tk.Tk()

        window.title("Hotdog or Not Hotdog")
        window.geometry('400x600')

        text_font = font.Font(size=18, family="Helvetica Neue")
        welcome_text = tk.Label(window, text="Hot Dog or Not Hot Dog", font=text_font)
        welcome_text.pack()

        instructions_text = tk.Label(window, text="\n\nUse a neural network built with Tensorflow\n"
            "to identify photos containing hot dogs")
        instructions_text.pack(fill=tk.X)

        select_btn = tk.Button(window, text="Select", bg="#0063B1", fg="white", width=5, height=1)
        select_btn.pack(pady=30)

        image_label = tk.Label(window)
        image_label.pack()

        select_btn.configure(command=lambda: select_image_click(image_label))
        window.mainloop()

    if __name__ == "__main__":
        run()
    ```

    <span data-ttu-id="c58aa-111">Il codice importante qui è la chiamata a ```subprocess.check_output```, che richiama il modello sottoposto a training eseguendo uno script Python denominato **label_image.py** disponibile nella cartella "scripts", passando l'immagine selezionata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="c58aa-111">The key code here is the call to ```subprocess.check_output```, which invokes the trained model by executing a Python script named **label_image.py** found in the "scripts" folder, passing in the image that the user selected.</span></span> <span data-ttu-id="c58aa-112">Questo script proviene dal repository clonato nell'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="c58aa-112">This script came from the repo that you cloned in the previous exercise.</span></span>

1. <span data-ttu-id="c58aa-113">Usare il motore di ricerca preferito per trovare alcune immagini di cibo, alcune contenenti hot dog e altre no.</span><span class="sxs-lookup"><span data-stu-id="c58aa-113">Use your favorite search engine to find a few food images — some containing hot dogs and some not.</span></span> <span data-ttu-id="c58aa-114">Scaricare queste immagini e archiviarle in una posizione a propria scelta nel file system della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="c58aa-114">Download these images and store them in the location of your choice in the VM's file system.</span></span>

1. <span data-ttu-id="c58aa-115">Selezionare il comando di Visual Studio Code **Visualizza > Terminale integrato** per aprire un terminale integrato.</span><span class="sxs-lookup"><span data-stu-id="c58aa-115">Use Visual Studio Code's **View > Integrated Terminal** command to open an integrated terminal.</span></span> <span data-ttu-id="c58aa-116">Eseguire quindi il comando seguente nel terminale integrato per eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="c58aa-116">Then, execute the following command in the integrated terminal to run the app:</span></span>

     ```bash
     python classify.py
     ```

1. <span data-ttu-id="c58aa-117">Fare clic sul pulsante **Select** dell'app e selezionare una delle immagini con hot dog scaricate nel passaggio 3.</span><span class="sxs-lookup"><span data-stu-id="c58aa-117">Click the app's **Select** button and pick one of the hot-dog images you downloaded in Step 3.</span></span> <span data-ttu-id="c58aa-118">Attendere che venga visualizzata una finestra di messaggio che indica se l'immagine contiene un hot dog.</span><span class="sxs-lookup"><span data-stu-id="c58aa-118">Wait for a message box to appear, indicating whether the image contains a hot dog.</span></span> <span data-ttu-id="c58aa-119">Il modello rileva correttamente l'immagine?</span><span class="sxs-lookup"><span data-stu-id="c58aa-119">Did the model get it correct?</span></span>

    > <span data-ttu-id="c58aa-120">Se vengono visualizzati messaggi di errore riguardanti un driver del kernel mancate nella finestra del terminale durante l'elaborazione di un'immagine, è possibile ignorarli.</span><span class="sxs-lookup"><span data-stu-id="c58aa-120">If you see error messages regarding a missing kernel driver in the terminal window when you process an image, you can safely ignore them.</span></span> <span data-ttu-id="c58aa-121">Sono il risultato del fatto che la Data Science VM non contiene una GPU virtuale.</span><span class="sxs-lookup"><span data-stu-id="c58aa-121">They result from the fact that the Data Science VM does not contain a virtual GPU.</span></span>

    ![Selezione di un'immagine](../media-draft/4-select-image.png)

1. <span data-ttu-id="c58aa-123">Ripetere il passaggio precedente usando un'immagine che non contiene un hot dog.</span><span class="sxs-lookup"><span data-stu-id="c58aa-123">Repeat the previous step using an image that doesn't contain a hot dog.</span></span> <span data-ttu-id="c58aa-124">Il modello risponde correttamente questa volta?</span><span class="sxs-lookup"><span data-stu-id="c58aa-124">Was the model right this time?</span></span>

<span data-ttu-id="c58aa-125">Continuare inserendo le immagini di cibo nell'app fino a quando non si verifica che è in grado di identificare le immagini contenenti hot dog.</span><span class="sxs-lookup"><span data-stu-id="c58aa-125">Continue feeding food images into the app until you're satisfied that it can identify images containing hot dogs.</span></span> <span data-ttu-id="c58aa-126">Non aspettarsi che il risultato sia sempre corretto, ma che lo sia *la maggior parte* delle volte.</span><span class="sxs-lookup"><span data-stu-id="c58aa-126">Don't expect it to be right 100% of the time, but do expect it to be right *most* of the time.</span></span>