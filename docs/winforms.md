# Decoupling Winforms

### WinForm.cs

    Button
    Property: ActionHandler<ArgumentClass> NavigateToOtherWindow
    Method OnButton.click event {
	    NavigateToOtherWindow(new ArgumentClass(){
		    Useful info
	    }
    }


The event trigger of the button is coupled to an actionhandler with an interface, but the actionhandler has no implementation. The view only knows when to call the handler and with what info to call it, but it does not know the implementation or provides no implementation.

### Controller.cs


    Method Navigate(ArgumentClass args){} --> implementation of logic by controller
    WinForm.NavigateToOtherWindow = Navigate


The controller will provide the implementation and will add this implementation to the actionhandler of the view. This way, the implementation logic is fully decoupled from the view and can be adjusted if necessary.