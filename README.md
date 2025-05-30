this is my custom flash message partial 
<!-- app/views/shared/_flash.html.erb -->
<% flash.each do |type, msg| %>
  <% if type == "alert" %>
    <% bg, fg, br = "#f8d7da","#721c24","#f5c6cb" %>
  <% else %>
    <% bg, fg, br = "#d1ecf1","#0c5460","#bee5eb" %>
  <% end %>
  <div
    data-controller="flash"
    role="alert"
    style="
      position: fixed;
      margin-left: 387px;
      margin-top: -62px;
      min-width:200px; max-width:320px;
      padding:0.75rem 1rem;
      border-radius:4px;
      box-shadow:0 2px 6px rgba(0,0,0,0.2);
      opacity:0; transform:translateY(-10px);
      transition:opacity 0.3s ease, transform 0.3s ease;
      z-index:1050;
      display:flex; justify-content:space-between;
      align-items:center;
      background-color: <%= bg %>;
      color:#c80b1c;
      border:1px solid <%= br %>;
    "
  >
    <%= msg %>
    <button
      type="button"
      aria-label="Close"
      data-action="click->flash#dismiss"
      style="
        background:transparent; border:none;
        font-size:1.2rem; line-height:1;
        color:currentColor; cursor:pointer;
      "
    >&times;</button>
  </div>
<% end %>


and im rendering in my layout file but in some page its showing empty flash  toaster i dont know why in my lsd_decisions/new page its showing empty why this is my new method 

def new
    # Initialize session variables with defaults if nil
    session[:user_decisionNavigation] ||= Array.new(17)
    session[:user_decisionNewTools] ||= Array.new(3)

    # Initialize instance variabl+es for the view
    @lsd_decision = LsdDecision.new
    puts "@lsd_decision>>>>>>>>>>>: #{@lsd_decision.inspect}"

    @lsd_checklist = LsdChecklist.list_all(I18n.locale) # Use I18n.locale for localization
    @back_url = lsd_decisions_path
    puts "@lsd_checklist>>>>>>>>>>>: #{@lsd_checklist.inspect}"

    @user_login = session[:user_login]
    @decision_id = session[:user_decision]
    @decision_user = session[:user_id]
    @selected_decision_profile = 1
    @jquery = "new_lsd_decision" # Use snake_case for variable names in Rails 8

    # Hidden tools default values
    @hidden_tools = {
      hide_scores: 0,
      hide_lifematch: 0,
      hide_pointofview: 0
    }

    @simon_bubble_says = "decisionprofile" # Use snake_case for variable names

    # Assign checklist from params if provided
    @ch = params[:ch].present? ? params[:ch] : -1

    @decision_profile = "#" # Preset to #

    # Build breadcrumb navigation
    @breadcrumb = helpers.safe_join([
      view_context.link_to(t("Breadcrumbs.my_decisions"), lsd_decisions_path),
      view_context.image_tag("arrow.gif", alt: "..", width: 5, height: 9),
      view_context.link_to(t("Breadcrumbs.decision_profile"), "#")
    ], " ")

    respond_to do |format|
      format.html # new.html.erb
    end
  end
