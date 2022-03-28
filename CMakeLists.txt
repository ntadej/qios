cmake_minimum_required(VERSION 3.16)

project(qios
    VERSION "6.3.0.0"
    LANGUAGES CXX C
)

# Make sure we use the fixed BASE argument of qt_add_resource.
set(QT_USE_FIXED_QT_ADD_RESOURCE_BASE TRUE)

find_package(Qt6 ${PROJECT_VERSION} CONFIG REQUIRED COMPONENTS BuildInternals Core Gui OpenGL) # special case

qt_prepare_standalone_project()

#####################################################################
## QIOSIntegrationPlugin Plugin:
#####################################################################

qt_internal_add_plugin(QIOSIntegrationPlugin
    OUTPUT_NAME qios
    DEFAULT_IF ${QT_QPA_DEFAULT_PLATFORM} MATCHES ios # special case
    PLUGIN_TYPE platforms
    SOURCES
        plugin.mm
        qiosapplicationdelegate.h qiosapplicationdelegate.mm
        qiosapplicationstate.h qiosapplicationstate.mm
        qiosbackingstore.h qiosbackingstore.mm
        qioscontext.h qioscontext.mm
        qioseventdispatcher.h qioseventdispatcher.mm
        qiosglobal.h qiosglobal.mm
        qiosinputcontext.h qiosinputcontext.mm
        qiosintegration.h qiosintegration.mm
        qiosplatformaccessibility.h qiosplatformaccessibility.mm
        qiosscreen.h qiosscreen.mm
        qiosservices.h qiosservices.mm
        qiostextresponder.h qiostextresponder.mm
        qiostheme.h qiostheme.mm
        qiosviewcontroller.h qiosviewcontroller.mm
        qioswindow.h qioswindow.mm
        quiaccessibilityelement.h quiaccessibilityelement.mm
        quiview.h quiview.mm
        uistrings_p.h uistrings.cpp
    LIBRARIES
        ${FWAudioToolbox}
        ${FWFoundation}
        ${FWMetal}
        ${FWQuartzCore}
        ${FWUIKit}
        Qt::CorePrivate
        Qt::GuiPrivate
)

# special case begin
qt_disable_apple_app_extension_api_only(QIOSIntegrationPlugin)
# special case end
#### Keys ignored in scope 2:.:.:kernel.pro:<TRUE>:
# OTHER_FILES = "quiview_textinput.mm" "quiview_accessibility.mm"

## Scopes:
#####################################################################

qt_internal_extend_target(QIOSIntegrationPlugin CONDITION QT_FEATURE_opengl
    LIBRARIES
        Qt::OpenGLPrivate
)

qt_internal_extend_target(QIOSIntegrationPlugin CONDITION NOT TVOS
    SOURCES
        qiosclipboard.h qiosclipboard.mm
        qiosdocumentpickercontroller.h qiosdocumentpickercontroller.mm
        qiosfiledialog.h qiosfiledialog.mm
        qiosmenu.h qiosmenu.mm
        qiosmessagedialog.h qiosmessagedialog.mm
        qiostextinputoverlay.h qiostextinputoverlay.mm
    LIBRARIES
        ${FWAssetsLibrary}
)

#### Keys ignored in scope 6:.:.:kernel.pro:NOT TARGET___equals____ss_QT_DEFAULT_QPA_PLUGIN:
# PLUGIN_EXTENDS = "-"
add_subdirectory(optional)
