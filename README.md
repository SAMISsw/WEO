# WEO
Youtube
```swift
import Cocoa
import SwiftUI
import CEFswift

@main
@ApplicationDelegateAdaptor(AppDelegate.self)
struct MyApp {

}

class AppDelegate: NSObject, NSApplicationDelegate {
    var window: NSWindow!

    func applicationDidFinishLaunching(_ aNotification: Notification) {
        let cefSettings = CEFSettings()
        cefSettings.noSandbox = true
        
        CEFProcessUtils.initializeMain(args: ProcessInfo.processInfo.arguments, settings: cefSettings)
        
        let contentView = ContentView()

        window = NSWindow(
            contentRect: NSScreen.main!.frame,
            styleMask: [.titled, .closable, .resizable, .miniaturizable],
            backing: .buffered,
            defer: false)
        window.center()
        window.title = "CEF.swift Browser"
        window.contentView = NSHostingView(rootView: contentView)
        window.makeKeyAndOrderFront(nil)
        window.setFrame(NSScreen.main!.frame, display: true)
    }

    func applicationWillTerminate(_ aNotification: Notification) {
    }
}

struct ContentView: View {
    var body: some View {
        WebView(url: URL(string: "https://www.youtube.com")!)
            .edgesIgnoringSafeArea(.all)
    }
}

struct WebView: NSViewRepresentable {
    let url: URL

    func makeNSView(context: Context) -> NSView {
        return CEFBrowserView(url: url)
    }

    func updateNSView(_ nsView: NSView, context: Context) {
    }
}

class CEFBrowserView: NSView, CEFLifeSpanHandler {
    var browser: CEFBrowser!
    var client: CEFClient!

    init(url: URL) {
        super.init(frame: .zero)
        
        let windowInfo = CEFWindowInfo()
        windowInfo.windowName = "CEF Browser"
        windowInfo.bounds = self.bounds

        let browserSettings = CEFBrowserSettings()
        browserSettings.windowlessRenderingEnabled = true
        
        let client = CEFClient()
        client.lifeSpanHandler = self
        self.client = client

        browser = CEFBrowser(windowInfo: windowInfo, url: url.absoluteString, settings: browserSettings, client: client)
    }

    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    override func layout() {
        super.layout()
        browser.host?.wasResized()
    }
    
    func onAfterCreated(browser: CEFBrowser) {
    }

    func doClose(browser: CEFBrowser) -> CEFOnCloseAction {
        return .proceed
    }

    func onBeforeClose(browser: CEFBrowser) {
    }

    func onBeforePopup(browser: CEFBrowser,
                       frame: CEFFrame,
                       targetURL: URL,
                       targetFrameName: String,
                       targetDisposition: CEFWindowOpenDisposition,
                       userGesture: Bool,
                       popupFeatures: CEFPopupFeatures,
                       windowInfo: inout CEFWindowInfo,
                       client: inout CEFClient,
                       settings: inout CEFBrowserSettings,
                       jsAccess: inout CEFJavaScriptDialogSettings) -> CEFOnBeforePopupAction {
        windowInfo.windowName = "CEF Popup"
        windowInfo.bounds = self.bounds
        client = CEFClient()
        client.lifeSpanHandler = self
        return .allow
    }
}

```
```
import Cocoa
import SwiftUI
import CEFswift

@main
class AppDelegate: NSObject, NSApplicationDelegate {
    var window: NSWindow!

    func applicationDidFinishLaunching(_ aNotification: Notification) {
        let cefSettings = CEFSettings()
        cefSettings.noSandbox = true
        
        CEFProcessUtils.initializeMain(args: ProcessInfo.processInfo.arguments, settings: cefSettings)
        
        let contentView = ContentView()

        window = NSWindow(
            contentRect: NSScreen.main!.frame,
            styleMask: [.titled, .closable, .resizable, .miniaturizable],
            backing: .buffered,
            defer: false)
        window.center()
        window.title = "CEF.swift Browser"
        window.contentView = NSHostingView(rootView: contentView)
        window.makeKeyAndOrderFront(nil)
        window.setFrame(NSScreen.main!.frame, display: true)
    }

    func applicationWillTerminate(_ aNotification: Notification) {
    }
}

struct ContentView: View {
    var body: some View {
        WebView(url: URL(string: "https://www.youtube.com")!)
            .edgesIgnoringSafeArea(.all)
    }
}

struct WebView: NSViewRepresentable {
    let url: URL

    func makeNSView(context: Context) -> NSView {
        return CEFBrowserView(url: url)
    }

    func updateNSView(_ nsView: NSView, context: Context) {
    }
}

class CEFBrowserView: NSView, CEFLifeSpanHandler {
    var browser: CEFBrowser!
    var client: CEFClient!

    init(url: URL) {
        super.init(frame: .zero)
        
        let windowInfo = CEFWindowInfo()
        windowInfo.windowName = "CEF Browser"
        windowInfo.bounds = self.bounds

        let browserSettings = CEFBrowserSettings()
        browserSettings.windowlessRenderingEnabled = true
        
        let client = CEFClient()
        client.lifeSpanHandler = self
        self.client = client

        browser = CEFBrowser(windowInfo: windowInfo, url: url.absoluteString, settings: browserSettings, client: client)
    }

    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    override func layout() {
        super.layout()
        browser.host?.wasResized()
    }
    
    func onAfterCreated(browser: CEFBrowser) {
    }

    func doClose(browser: CEFBrowser) -> CEFOnCloseAction {
        return .proceed
    }

    func onBeforeClose(browser: CEFBrowser) {
    }

    func onBeforePopup(browser: CEFBrowser,
                       frame: CEFFrame,
                       targetURL: URL,
                       targetFrameName: String,
                       targetDisposition: CEFWindowOpenDisposition,
                       userGesture: Bool,
                       popupFeatures: CEFPopupFeatures,
                       windowInfo: inout CEFWindowInfo,
                       client: inout CEFClient,
                       settings: inout CEFBrowserSettings,
                       jsAccess: inout CEFJavaScriptDialogSettings) -> CEFOnBeforePopupAction {
        windowInfo.windowName = "CEF Popup"
        windowInfo.bounds = self.bounds
        client = CEFClient()
        client.lifeSpanHandler = self
        return .allow
    }
}
```
WEO NAVER
```swift
import SwiftUI
import WebKit
import Network
import AVKit
import AVFoundation

extension String {
    func toBase64() -> String {
        return Data(self.utf8).base64EncodedString()
    }
}

struct ContentView: View {
    @StateObject private var viewModel = BrowserViewModel()
    @State private var showNewWindow = false
    @State private var currentPageTitle: String = ""
    
    var body: some View {
        VStack {
            Text("WEO")
                .font(.custom("Impact", size: 40))
                .foregroundColor(.white)
                .padding()
                .frame(maxWidth: .infinity, alignment: .center)
            
            HStack {
                TextField("Digite a URL ou pesquisa", text: $viewModel.urlString, onCommit: { viewModel.loadURL() })
                    .textFieldStyle(.roundedBorder)
                    .padding()
                    .frame(minHeight: 50)
                    .background(Color.white.opacity(0.2))
                    .cornerRadius(10)

                Button(action: viewModel.goBack) {
                    Image(systemName: "arrow.left.circle.fill")
                        .font(.title)
                        .foregroundColor(.white)
                }
                .disabled(!viewModel.canGoBack)
                .padding()

                Button(action: viewModel.goForward) {
                    Image(systemName: "arrow.right.circle.fill")
                        .font(.title)
                        .foregroundColor(.white)
                }
                .disabled(!viewModel.canGoForward)
                .padding()
            }

            ScrollView(.horizontal) {
                HStack {
                    ForEach(viewModel.openWindows, id: \.id) { window in
                        Button(action: {
                            viewModel.switchToWindow(window)
                        }) {
                            Text(window.title)
                                .foregroundColor(.blue)
                                .padding()
                        }
                    }
                }
            }
            
            WebView(viewModel: viewModel, currentPageTitle: $currentPageTitle)
                .frame(maxHeight: .infinity)

            HStack {
                Button(action: { showNewWindow.toggle() }) {
                    Image(systemName: "app.fill")
                        .font(.title)
                        .foregroundColor(.white)
                }
                .padding()

                Button(action: { viewModel.savePage() }) {
                    Image(systemName: "square.and.arrow.down")
                        .font(.title)
                        .foregroundColor(.white)
                }

                Button(action: { viewModel.saveMedia() }) {
                    Image(systemName: "photo.fill")
                        .font(.title)
                        .foregroundColor(.white)
                }
            }
            .padding()
            
            if showNewWindow {
                WindowView(showWindow: $showNewWindow, currentPageTitle: currentPageTitle)
            }
        }
        .background(Color.blue)
        .cornerRadius(20)
        .padding()
        .onAppear {
            viewModel.loadGoogleSearch()
        }
    }
}

struct WebView: UIViewRepresentable {
    @ObservedObject var viewModel: BrowserViewModel
    @Binding var currentPageTitle: String

    func makeUIView(context: Context) -> WKWebView {
        let webview = WKWebView()
        webview.navigationDelegate = context.coordinator
        return webview
    }

    func updateUIView(_ webview: WKWebView, context: Context) {
        if viewModel.isOnline {
            if let url = viewModel.currentURL {
                webview.load(URLRequest(url: url))
            }
        } else {
            webview.loadHTMLString(viewModel.offlineHTML, baseURL: nil)
        }
    }

    func makeCoordinator() -> Coordinator {
        return Coordinator(viewModel: viewModel, currentPageTitle: $currentPageTitle)
    }

    class Coordinator: NSObject, WKNavigationDelegate {
        var viewModel: BrowserViewModel
        @Binding var currentPageTitle: String

        init(viewModel: BrowserViewModel, currentPageTitle: Binding<String>) {
            self.viewModel = viewModel
            self._currentPageTitle = currentPageTitle
        }

        func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
            webView.evaluateJavaScript("document.title") { title, _ in
                if let pageTitle = title as? String {
                    self.currentPageTitle = pageTitle
                }
            }

            viewModel.updateNavigationState(webView)
        }
    }
}

struct WindowView: View {
    @Binding var showWindow: Bool
    var currentPageTitle: String

    var body: some View {
        VStack {
            Text("Página atual: \(currentPageTitle)")
                .font(.subheadline)
                .padding()

            Button(action: {
                showWindow = false
            }) {
                Image(systemName: "xmark.circle.fill")
                    .font(.title)
                    .foregroundColor(.red)
            }
            .padding()
        }
        .background(Color.white)
        .cornerRadius(15)
        .frame(width: 300, height: 300)
        .shadow(radius: 10)
        .padding()
    }
}

class BrowserViewModel: ObservableObject {
    @Published var currentURL: URL?
    @Published var urlString: String = ""
    @Published var offlineHTML: String = ""
    @Published var canGoBack = false
    @Published var canGoForward = false
    @Published var isOnline: Bool = true
    @Published var savedPages = [String: String]()
    @Published var mediaFiles = [String: MediaData]()
    @Published var openWindows: [BrowserWindow] = []
    @Published var navigationHistory: [String] = []
    @Published var userSettings: [String: Any] = [:]
    @Published var userData: [String: String] = [:]

    private var webView: WKWebView?
    private var monitor = NWPathMonitor()

    init() {
        monitor.pathUpdateHandler = { path in
            DispatchQueue.main.async {
                self.isOnline = path.status == .satisfied
            }
        }
        let queue = DispatchQueue(label: "Monitor")
        monitor.start(queue: queue)
    }

    func loadURL() {
        if isValidURL(urlString) {
            if let url = URL(string: urlString) {
                currentURL = url
                addWindow(withTitle: "Página \(urlString)")
            }
        } else {
            performGoogleSearch(query: urlString)
        }
    }

    func isValidURL(_ string: String) -> Bool {
        return string.lowercased().hasPrefix("http://") || string.lowercased().hasPrefix("https://")
    }

    func performGoogleSearch(query: String) {
        let searchQuery = query.addingPercentEncoding(withAllowedCharacters: .urlQueryAllowed) ?? query
        let googleURL = "https://www.google.com/search?q=\(searchQuery)"
        if let url = URL(string: googleURL) {
            currentURL = url
            addWindow(withTitle: "Busca Google")
        }
    }

    func loadGoogleSearch() {
        urlString = "https://www.google.com"
        loadURL()
    }

    func addWindow(withTitle title: String) {
        let window = BrowserWindow(title: title, url: currentURL ?? URL(string: "https://www.google.com")!)
        openWindows.append(window)
        navigationHistory.append(title)
        saveNavigationHistory()
    }

    func switchToWindow(_ window: BrowserWindow) {
        currentURL = window.url
    }

    func savePage() {
        guard let currentURL = currentURL else { return }
        UserDefaults.standard.set(offlineHTML, forKey: currentURL.absoluteString)
    }

    func saveMedia() {
        webView?.evaluateJavaScript("""
            var media = document.querySelectorAll('video, audio, img, source');
            var mediaArray = [];
            media.forEach(function(item) {
                if (item.src) mediaArray.push(item.src);
            });
            return mediaArray;
        """) { (media, _) in
            if let mediaArray = media as? [String] {
                self.downloadAndSaveMedia(mediaArray)
            }
        }
    }

    func downloadAndSaveMedia(_ mediaArray: [String]) {
        for media in mediaArray {
            if let url = URL(string: media) {
                downloadFile(from: url) { success in
                    if success {
                        let fileName = url.lastPathComponent
                        let mediaData = MediaData(fileName: fileName, filePath: self.getDocumentsDirectory().appendingPathComponent(fileName).path)
                        self.mediaFiles[fileName] = mediaData
                    }
                }
            }
        }
    }

    func downloadFile(from url: URL, completion: @escaping (Bool) -> Void) {
        let downloadTask = URLSession.shared.downloadTask(with: url) { (url, response, error) in
            guard let url = url, error == nil else {
                completion(false)
                return
            }

            do {
                let destinationURL = self.getDocumentsDirectory().appendingPathComponent(url.lastPathComponent)
                try FileManager.default.moveItem(at: url, to: destinationURL)
                completion(true)
            } catch {
                completion(false)
            }
        }
        downloadTask.resume()
    }

    func getDocumentsDirectory() -> URL {
        let paths = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
        return paths[0]
    }

    func updateNavigationState(_ webView: WKWebView) {
        canGoBack = webView.canGoBack
        canGoForward = webView.canGoForward
    }

    func saveNavigationHistory() {
        UserDefaults.standard.set(navigationHistory, forKey: "navigationHistory")
    }

    func loadUserData() {
        if let storedData = UserDefaults.standard.dictionary(forKey: "userData") {
            userData = storedData as? [String: String] ?? [:]
        }
    }
}

struct BrowserWindow: Identifiable {
    var id = UUID()
    var title: String
    var url: URL
}

struct MediaData {
    var fileName: String
    var filePath: String
}

```
