FROM scottyhardy/docker-wine:stable-10.0

COPY data /home/wineuser/data

ENV USE_XVFB "yes"
ENV XVFB_SERVER ":95"
ENV XVFB_SCREEN "0"
ENV XVFB_RESOLUTION "1024x768x8"
ENV DISPLAY ":95"
ENV WINEARCH "win32"

RUN (RDP_SERVER=invalid /usr/bin/entrypoint || true) && \
  export DISPLAY=:0 && \
  (Xvfb $DISPLAY -screen 0 1024x768x24 > /dev/null 2>&1 &) && \
  sudo --user=wineuser WINEARCH=win32 winetricks --unattended -q --force winxp cmd dotnet35 && \
  sudo --user=wineuser winetricks winxp && \
  sudo --user=wineuser wine /home/wineuser/data/windows-en-us-kb971513.exe /quiet && \
  mv /home/wineuser/data /home/wineuser/.wine/drive_c && \
  cd /home/wineuser/.wine/drive_c/data && \
  mkdir ahk && unzip ahk.zip -d ahk && \
  mkdir nextion && unzip nextion.zip -d nextion && \
  unzip pulovers.zip && \
  unzip uiaviewer.zip -d ahk && \
  mv ahk/UIAutomation-* ahk/uia && \
  chown -R wineuser:wineuser /home/wineuser/.wine/drive_c/data

ENTRYPOINT ["/usr/bin/entrypoint"]
