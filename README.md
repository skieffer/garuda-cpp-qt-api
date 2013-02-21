Garuda API for C++ / Qt applications
====================================

### Activate / Register ###

To activate and register your gadget, you simply construct an instance of
the GarudaClient class. The constructor signature and doctext is as follows:

    /**
     * parent: a QObject, such as a QApplication object, which is to "own" this
     *   client object. May be set to 0 if there is no obvious parent.
     *
     * gadgetName: the name of the gadget
     *
     * gadgetUUID: the UUID of the gadget
     *
     * providerName: the name and affiliation of the provider of the gadget
     *
     * description: a description of the gadget
     *
     * categoryList: a list of Garuda-approved category names, to which this
     *   gadget belongs
     *
     * inputFiles: an even-length list of strings, giving extension-format pairs
     *   for files that this gadget can open.
     *   For example, [ "xml", "sbml" ]. If you provide an odd number of strings,
     *   the final entry will simply be ignored.
     *
     * outputFiles: same format as inputFiles, this time specifying the kinds of
     *   files that this gadget creates.
     *
     * iconPath: absolute path to icon file
     *
     * screenshotPaths: list of absolute paths to screenshot files
     *
     * launchCommand: the full command line string that launches the gadget
     */
    GarudaClient(QObject *parent, QString gadgetName,
                 QString gadgetUUID, QString providerName,
                 QString description, QList<QString> categoryList,
                 QList<QString> inputFiles, QList<QString> outputFiles,
                 QString iconPath, QList<QString> screenshotPaths,
                 QString launchCommand)


### GetCompatibleGadgetList ###

To request a list of compatible gadgets, you make a function call:

    showCompatibleSoftwareFor(QString extension, QString format);

naming the file extension and format that you are interested in.
For example:

    QString extension = "xml";
    QString format = "sbml";

When the information is ready, it will be returned using the GarudaClient's
Qt SIGNAL

    void showCompatibleSoftwareResponse(const QVariantMap& response);

Therefore, in order to receive the signal your application should implement a slot
and connect it using a line like:

    connect (m_garuda_client, SIGNAL(showCompatibleSoftwareResponse(QVariantMap)),
             this, SLOT(myShowCompatibleSoftwareResponse(QVariantMap)));

where m_garuda_client is the GarudaClient instance that you constructed, and
myShowCompatibleSoftwareResponse is your function which accepts a QVariantMap describing
the available compatible gadgets.


### SendData / LoadData ###

When you want to ask another gadget to open a file, you make a function call:

    loadFileIntoSoftware(QFileInfo fileInfo, QString softwareId, QString softwareVersion);

where ....


When another gadget asks you to open a file, GarudaClient will emit the signal:

    void fileOpenRequest(const QString& filename);

So in your application, include a line like:

    connect( m_garuda_client,
             SIGNAL(fileOpenRequest(QString)),
             this,
             SLOT(myFileOpenRequest(QString))
    );

which connects to a function you define, called myFileOpenRequest.


### LoadGadget ###

Not yet supported ... Check back soon!


