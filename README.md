# imgui_tabs
Tab module for imgui. Should be relatively forwardly compatible. 

Code like this: 
```
ImGui::BeginTabBar("Settings#left_tabs_bar");
if (ImGui::AddTab("General")){
    bool fullscreen = mGUICfg->fullScreen.value;
    if(ImGui::Checkbox("Fullscreen Mode",&fullscreen)){
        mGUICfg->toggleFullscreenMode = true;
    }
    if(ImGui::Checkbox("Enable Multisampling",&mGUICfg->enableMultisampling.value)){
        mGUICfg->settingsChanged = true;
    }

    if(ImGui::SliderInt("MSAA Count",(int*)&mGUICfg->multisampleCount.value,mGUICfg->multisampleCount.lowerLimit,mGUICfg->multisampleCount.upperLimit))
        mGUICfg->settingsChanged = true;
}
if (ImGui::AddTab("GUI")){
    ImGui::Text("Tab 2");
}
if (ImGui::AddTab("Tab Name")){
    ImGui::Text("Tab 3");
}
if (ImGui::AddTab("Tab Name")){
    ImGui::Text("Tab 4");
}
ImGui::EndTabBar();

ImGui::Dummy(ImVec2(0,20));

ImGui::BeginTabBar("#Additional Parameters");
float value = 0.0f;
if (ImGui::AddTab("Tab Name2")){
    ImGui::SliderFloat("Slider",&value,0,1.0f);
}
if (ImGui::AddTab("Tab Name3")){
    ImGui::Text("Tab 2");
}
if (ImGui::AddTab("Tab Name4")){
    ImGui::Text("Tab 3");
}
if (ImGui::AddTab("Tab Name5")){
    ImGui::Text("Tab 4");
}
ImGui::EndTabBar();
```
Yields results like this:

![alt text](http://i.imgur.com/6PSK9NL.gif "Animated Tabs")

  Usage:
     
     To start a tab bar, use: ImGui::BeginTabBar(const char* label)
          -> The label can be blank if you want, but you MUST!! pass a string of some length. Use the '#' character to
              give it an ID without a label.
 
      Add tabs by using:
          if (ImGui::AddTab("Tab Name")){
                 < Insert other draw commands here >
          }
          -> This is much like collapsing header. If the tab is active, then AddTab will return true, allowing drawing
              to occur.
 
      End the tab bar by using ImGui::EndTabBar()
 
      And that's it! You can dynamically add or remove tabs, the bar will reset the ID and tab label vectors appropriately,
      but it DOESN'T continuously allocate/deallocate them in memory every time (unless the CRC32 hashes of the tab or tab bar change,
      computed from their respective labels).
 
      Notes:
          * You can change the rounding by altering:
                  "static constexpr const float rounding = 6.0f;"
             in the _drawTabBarTop function. Alternatively, you can add it as a parameter, but to save on memory being passed
             around, and since I will be using 6 as a constant, I didn't include this.
 
          * The constexpr:
                  "static constexpr const float shrink = 1.0f;"
             in the _drawTabBarTop function needs to be set to the width of your borders.
 
          * I added custom colors and a new prototype for GetColorU32() to acquire these colors. You can change them here.
 
          * I added extra operator functions for ImVec2 and ImVec4, _YOU MUST_ define IMGUI_DEFINE_MATH_OPERATORS in
             imgui_internal.h for this to work.
 
          * Please attribute my work if used or added, along with the original ImGui license/attribution.
 
 
